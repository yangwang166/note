# AWS Developer Associate

* RDS do not directly supported automated scaling to manage a higher load
* An S3 bucket without versioning, version ID will be Null
* When you try to delete a SQS queue, even it has un received message, the queue will be deleted
* If DynamoDB application performs more reads or writes than provisioned capacity, it will be throttled and you will receive 400 error codes
* SQS cannot guarantee that you receive message in order
* When you attached two private IPs to the ec2 instance, you can use ec2-net-utils package to configure the additional network interface(IP address) and update the routing table
* With VPC the user can specify multiple private IP address for this instance.
* dynamodb is a nosql database
* SQS supports unlimited number of queues and messages
* You can assign any private IP address to ec2 instance if
  * part of the associated subnet's IP address range
  * not reserved by amazon for IP networking purpose
  * not currently assigned to another interface
* SQS queue name are limited to 80 characters, and queue names must be unique within an AWS account
* User can setup to have two sepreate public IPs and separately security group for one ec2 is to launch a VPC instance with two network interfaces. Assign a separate security and elastic IP(EIP) to them. When user has attached more than one network interface with an instance, AWS cannot assign public IPs to them.
* EIP: elastic IP address
* A good understanding between EIP and public IP address
  * https://serverfault.com/questions/708121/whats-the-difference-between-eip-and-public-ip-in-aws
* DynamoDB automatically replicates your data synchronously across multiple availability zones within an aws region to ensure HA and data durability
* message in SQS will be kept 4 days by default.
  * retention period between 1min to 2 weeks
* aws swf (simple work flow), the coordination logic in a workflow is contained in a software program called a Decider. A dedicer schedules activity tasks, provides input data to activity workers, processes events that arrive while the workflow is in progress, and utimately ends(or closes) the workflow when the objective has been completes
* When use change the ingress rule for the RDS security group, the initial status of the ingress rule is `Authorizing`, once the changes are propagated the rule status will change to `authorized`.
* User need to mount the EBS device before using `df -h`
* If the account owner only want to give ec2 access of only the US West region, he should create an IAM policy and define the region in the `condition`
* DynamoDB has no table size by default, it have  unlimited storage.
* When user launching RDS with Multi AZ, the user cannot provision the AZ, RDS is launched automatically instead
* AWS Elastic Beanstalk is designed to support multiple running environment, you can have integration testing env, pre-prod env, prod env, each env independently configured and running on it's own separate aws resource.
* If you want to detached a EBS which is the root device of an instance, the instance must be stopped first.
* SQS message size max is 8KB, send larger than 8kb will receive a HTTP code 400
* If we want the EC2 instance in us west to access RDS instance in us east, we need to configure the IP range of US west region instance as the ingress security rule of RDS
