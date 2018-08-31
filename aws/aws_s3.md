# AWS Simple Storage Service (S3)

S3 is the oldest service in aws

S3 provide developers and IT team with secure, durable, highly-scalable object storage. Amazon S3 is easy to use, with a simple web service interface to store and retrieve any amount of data from anywhere on the web.

Object-based storage
* pic
* pdf
* video

Contract to block-based storage
* install os
* install db
* install application

Basic
* File can be from 0 bytes to 5TB

* There is unlimited storage
  * pay by the G

* File are stored in Buckets
  * Bucket can be treat as a folder inside cloud
  * Bucket has a universal name, unique globally
  * each bucket you created, it has a website address(dns address)
  * url example: https://s3-ap-southeast-2.amazonaws.com/yangwang166/certificate_hdpca.pdf
  * If upload successful, you get HTTP 200 code
  * SLA (service level agreement)
    * Built for 99.99% availability for S3 platform
    * Amazon Guarantee 99.9% availability
    * Amazon guarantee 99.999999999% durability for S3 information (11x9)
      * it means you will not lose your data with 99.999999999%
  * Tiered Storage Available
  * Lifecycle Management
  * Versioning
  * Encryption
  * Secure you data using Access Control Lists and Bucket Policies
  * Storage Tier/classes
    * S3 standard:
      * 99.99% availability
      * 99.999999999% durability
      * stored redundantly across multiple devices(Disk) in multiple facilities(AZ), designed to sustain the loss of 2 facilities(AZ) concurrently.
    * S3 IA (infrequently accessed):
      * data that is accessed less frequently, but requires rapid access when needed.
      * Lower fee than S3, but you are charged a retrieval fee
      * store also cross multiple facilities and devices
    * S3 One Zone - IA:
      * cheaper than IA, in 1 zone
    * Glacier
      * cheapest
      * have 3 Model
        * Expedited: 5 min to restore
        * Standard: 3-5 hours to restore
        * Bulk: 5-12 hours to restore

![S3 Tiers](images/aws_s3/s3_tiers.png)

https://aws.amazon.com/s3/storage-classes/




## Data Consistency Model for S3

* Read after Write consistency for PUTS of new Object
  * like upload a file, after write it to aws, you can read it, usually take milliseconds
* Eventual Consistency for overwrite PUTS and DELETES(can take some time to propagate)
  * update file, change a file, read it, you may got two result, the new one and the old one, but eventually you will only get the latest file, because s3 store file in different available zone, so need time to sync

## S3 is A Simple Key-value Store

* S3 is object based, objects consist of the following:
  * Key (this is simply the name of the object)
  * Value (this is simply the data and is made up of a sequence of bytes)
  * Version ID (important for versioning)
  * Metadata (Data about data you are storing)
  * Subresources
    * Access Control Lists
    * Torrent

## S3 - charges

* Storage
* Requests
* Storage Management Pricing
  * metadata
* Data Transfer pricing
  * from one region to another
* Transfer acceleration
  * fast, easy, secure transfer file over long distances between your end users and an S3 bucket
  * takes advantages of amazon cloudfront's globally distributed edge locations, data is routed to Amazon S3 over an optimized network path.

## S3 - Wrap up
* Remember that S3 is Object-based
  * allows you to upload files
* Files size: 0 Bytes to 5 TB
* Unlimited Storage
* Files stored in Buckets
* S3 is universal namespace. Name must be unique globally
  * eg: https://s3-ap-southeast-2.amazonaws.com/yangwang166/certificate_hdpcd_spark.pdf
* Read after write consistency for PUTs of new Objects
* Eventual Consistency for overwrite PUTs and DELETES (can take some to propagate)
* S3 Storage Class/Tiers
  * S3 (durable, immediately available, frequently accessed)
  * S3 - IA (durable, immediately available, infrequently accessed)
  * S3 One Zone - IA (even cheaper than IA, but only in 1 AZ)
  * Glacier - Archived data, where you can wait 3-5 hours before accessing
* core fundamental of s3 object
  * key(name)
  * value(data)
  * version id
  * metadata
  * subresources
    * ACL
    * Torrent
* Successful uploads will generate a HTTP 200 status code
* Read S3 FAQ:
   * https://aws.amazon.com/s3/faqs/
* Encryption
  * Client Side Encryption
  * Server Side Encryption
    * Amazon S3 managed keys(SSE-S3)
    * KMS(SSE-KMS)
    * Customer Provided Keys(SSE-C)
* Control access to buckets using either a bucket ACL or using Bucket Polices
* By default, buckets are private and all objects stored inside them are private

## S3-Versioning Tips

* Stores all versions of an object (including all writes and even if you delete an object)
* great backup tool
* once enabled, versioning cannot be disabled, only suspended
* integrates with lifecycle rules
* versioning's MFA delete capability, which use multi-factor authentication, can be used to provide an additional layer of security.

## Cross Region Replication

* useful in real life, eg store your bitcoin :-)
* Versioning must be enabled on both the source and destination buckets
* Regions must be unique
* Files in an existing bucket are not replicated automatically. All subsequent updated files will be replicated automatically
* You can not replicate to multiple buckets or use daisy chaining (at this time)
* Delete markers are replicated
* Deleting individual versions or delete markers will not be replicated
* Understand what Cross Region Replication is at a high level

## Lifecycle Management

we can add multiple transition
* 30 days from s3-standard to s3-standard-IA
* 60 days from s3-standard to glacier

![S3 lifecycle](images/aws_s3/s3_lifecycle.png)

Also, can expire object

![S3 lifecycle](images/aws_s3/s3_lifecycle_expire.png)

Then let's see the rule we create:

![S3 lifecycle](images/aws_s3/s3_lifecycle_rule.png)

* can be used in conjunction with versioning
* can be applied to current versions and previous versions
* following actions can now be done:
  * transition to the Standard - IA (30 days after creation)
  * archive to the Glacier Storage Class (30 days after IA)
* Permanently Delete
