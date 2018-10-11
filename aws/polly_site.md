# Using Polly to make a server-less service

## Arch

* ![Arch](../images/polly_site/arch.png)

## Create DynamoDB table

* table: posts
* key: id

Create S3 bucket
* datasci.guru
* text2mp3bucket


## Create Policy

* use following JSON to create myPollyLambdaPolicy

``` json
{
   "Version": "2012-10-17",
   "Statement": [
       {
           "Effect": "Allow",
           "Action": [
                "polly:SynthesizeSpeech",
                "dynamodb:Query",
                "dynamodb:Scan",
                "dynamodb:PutItem",
                "dynamodb:UpdateItem",
                "sns:Publish",
                "s3:PutObject",
                "s3:PutObjectAcl",
                "s3:GetBucketLocation",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
           ],
           "Resource": [
               "*"
           ]
       }
   ]
}
```

## Create role

* lambda to sns
* lambda to execute
* lambda to dynamodb
* lambda to s3
* Use the Policy we have created: myPollyLambdaRole

## Change S3 bucket policy

* To allow all object we put there is public

``` json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "PublicReadGetObject",
			"Effect": "Allow",
			"Principal": "*",
			"Action": [
				"s3:GetObject"
			],
			"Resource": [
				"arn:aws:s3:::datasci.guru/*"
			]
		}
	]
}
```

## Create SNS

* create a new topic
  * topic name: new_posts

## Create Lambda function

* Python2.7
* Role: myPollyLambdaRole
* Write code

``` python
import boto3
import os
import uuid

def lambda_handler(event, context):

    recordId = str(uuid.uuid4())
    voice = event["voice"]
    text = event["text"]

    print('Generating new DynamoDB record, with ID: ' + recordId)
    print('Input Text: ' + text)
    print('Selected voice: ' + voice)

    #Creating new record in DynamoDB table
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['DB_TABLE_NAME'])
    table.put_item(
        Item={
            'id' : recordId,
            'text' : text,
            'voice' : voice,
            'status' : 'PROCESSING'
        }
    )

    #Sending notification about new post to SNS
    client = boto3.client('sns')
    client.publish(
        TopicArn = os.environ['SNS_TOPIC'],
        Message = recordId
    )

    return recordId

```

* Add env
  * DB_TABLE_NAME: `posts`
  * SNS_TOPIC: `arn:aws:sns:ap-southeast-2:087659795860:new_posts`
* Add Test

``` json
{
	"voice" : "Joanna",
	"text" : "Hello Cloud Gurus!"
}
```

* Run the test and check the output and data in dynamodb

## Create Lambda function to convert text to voice

* Python2.7
* Set Role as myPollyLambdaRole
* Write code

``` python
import boto3
import os
from contextlib import closing
from boto3.dynamodb.conditions import Key, Attr

def lambda_handler(event, context):

    postId = event["Records"][0]["Sns"]["Message"]

    print "Text to Speech function. Post ID in DynamoDB: " + postId

    #Retrieving information about the post from DynamoDB table
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['DB_TABLE_NAME'])
    postItem = table.query(
        KeyConditionExpression=Key('id').eq(postId)
    )


    text = postItem["Items"][0]["text"]
    voice = postItem["Items"][0]["voice"]

    rest = text

    #Because single invocation of the polly synthesize_speech api can
    # transform text with about 1,500 characters, we are dividing the
    # post into blocks of approximately 1,000 characters.
    textBlocks = []
    while (len(rest) > 1100):
        begin = 0
        end = rest.find(".", 1000)

        if (end == -1):
            end = rest.find(" ", 1000)

        textBlock = rest[begin:end]
        rest = rest[end:]
        textBlocks.append(textBlock)
    textBlocks.append(rest)            

    #For each block, invoke Polly API, which will transform text into audio
    polly = boto3.client('polly')
    for textBlock in textBlocks:
        response = polly.synthesize_speech(
            OutputFormat='mp3',
            Text = textBlock,
            VoiceId = voice
        )

        #Save the audio stream returned by Amazon Polly on Lambda's temp
        # directory. If there are multiple text blocks, the audio stream
        # will be combined into a single file.
        if "AudioStream" in response:
            with closing(response["AudioStream"]) as stream:
                output = os.path.join("/tmp/", postId)
                with open(output, "a") as file:
                    file.write(stream.read())

    s3 = boto3.client('s3')
    s3.upload_file('/tmp/' + postId,
      os.environ['BUCKET_NAME'],
      postId + ".mp3")
    s3.put_object_acl(ACL='public-read',
      Bucket=os.environ['BUCKET_NAME'],
      Key= postId + ".mp3")

    location = s3.get_bucket_location(Bucket=os.environ['BUCKET_NAME'])
    region = location['LocationConstraint']

    if region is None:
        url_begining = "https://s3.amazonaws.com/"
    else:
        url_begining = "https://s3-" + str(region) + ".amazonaws.com/" \

    url = url_begining \
            + str(os.environ['BUCKET_NAME']) \
            + "/" \
            + str(postId) \
            + ".mp3"

    #Updating the item in DynamoDB
    response = table.update_item(
        Key={'id':postId},
          UpdateExpression=
            "SET #statusAtt = :statusValue, #urlAtt = :urlValue",                   
          ExpressionAttributeValues=
            {':statusValue': 'UPDATED', ':urlValue': url},
        ExpressionAttributeNames=
          {'#statusAtt': 'status', '#urlAtt': 'url'},
    )

    return

```

* Add env
* Increase timeout to 5mins
* Create SNS Trigger

## Create Lambda function to read from dynamodb

* As usual
* code

``` python
import boto3
import os
from boto3.dynamodb.conditions import Key, Attr

def lambda_handler(event, context):

    postId = event["postId"]

    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['DB_TABLE_NAME'])

    if postId=="*":
        items = table.scan()
    else:
        items = table.query(
            KeyConditionExpression=Key('id').eq(postId)
        )

    return items["Items"]
```

* test

``` json
{
  "postId": "*"
}
```

## Create API Gateway

* GET to trigger PostReader_GetPosts
* POST to trigger PostReader_NewPosts
* Enable CORS
* set GET Method Request
  * URL Query String Parameters
    * add postId
* set GET Integration Request
  * Mapping Template
    * When there are no templates defined(recommend): application/json
    * add:

``` json
{
    "postId" : "$input.params('postId')"
}
```

* deploy

## Upload site file into S3

* datasci.guru


## Build Alexa Skill

* Create Lambda function with Serverless Application Repository
* use: alexa-skills-kit-nodejs-factskill
* Go to https://github.com/alexa/skill-sample-nodejs-fact
* Go to models/en-US.json
* Will create a CloudFormation 
