# AWS Cli General

Install

```
pip install awscli
```

You should have a user with `AWS Access Key ID` & `AWS Secret Access Key`

Config aws cli

```
aws configure
AWS Access Key ID [****************]: AKIAXXXSNDXXXXXXQ
AWS Secret Access Key [****************]: skuxXXXXXXXXXXxUVXXXXJow
Default region name [None]: ap-southeast-2
Default output format [None]:
```

You can use `--profile name` to use different account
```
aws configure --profile name
aws s3 ls --profile name
```

Use aws cli

```
>aws s3 ls
2018-08-31 18:11:12 willwywang
2018-08-31 18:10:23 willwywang-singapore
2018-08-31 14:47:51 yangwang166
```

Copy all object between s3 buckets, using cross region replication.

```
>aws s3 cp --recursive s3://willwywang s3://willwywang-singapore
copy: s3://willwywang/hiaws.txt to s3://willwywang-singapore/hiaws.txt
copy: s3://willwywang/s3_tiers.png to s3://willwywang-singapore/s3_tiers.png
```

Copy local file to S3

```
aws s3 cp . s3://willwxxxxxte/ --recursive
``
