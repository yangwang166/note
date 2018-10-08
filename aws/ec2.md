# EC2

* EC2 instance types
![Types](../images/aws_ec2/ec2_instance_types.png)

* EC2 cost model
![Cost model](../images/aws_ec2/ec2_cost_model.png)
  * Spot
    * ![Spot](../images/aws_ec2/spot.png)

* FightDrMcPX
![How to memory](../images/aws_ec2/pic2.png)

* EBS
  * A virtual disk
  * a block device
  * are placed in a specific availability zone, are automatically replicated to protect you from the failure of a single component
  * Types
    * SSD
      * General Purpose SSD (GP2)
        * balance both price and performance
        * ratio of 3 IOPS (I/O per second) per GB with up to 10,000 IOPS and the ablility to burst up to 3000 IOPS for extended periods of time for volumes at 3334 GiB and above.
      * Provisioned IOPS SSD (IO1)
        * Designed for I/O intensive applications such as large relational or NoSQL DB
        * Use if you need more than 10,000 IOPS
        * Can provision up to 20,000 IOPS per volume
    * Magnetic
      * Throughput Optimized HDD(ST1)
        * Big data
        * Data warehouse
        * Log processing
        * Cannot be a boot volume
      * Cold HDD (SC1)
        * Lowest Cost Storage for infrequently accessed workloads
        * File Server
        * Cannot be a boot volume
      * Magnetic (Standard)
        * Lowest cost per gigabyte of all EBS volume types that is bootable.
        * Magnetic volumes are ideal fro workloads where data is accessed infrequently, and applications where the lowest storage cost is important
        * Previous Generation
    * ![EBS Type](../images/aws_ec2/ebs.png)

* EC2 latency test
  * https://www.cloudping.info/
  * In Perth AU
    *  ![Latency](../images/aws_ec2/ping.png)

* Lunch EC2 instance
  * Step 1: choose AMI
    * ![AMI](../images/aws_ec2/ami.png)
    * We need AWS command line tool, this AMI have many pre-build packages.
  * Step 2: choose instance type
    * Using t2.micro with free tier eligible
    * ![Type](../images/aws_ec2/type.png)
  * Step 3: config instance detail
    * ![Config](../images/aws_ec2/config.png)
    * One subnet only belong to 1 availability zone
  * Step 4: add storage
    * ![Storage](../images/aws_ec2/storage.png)
    * Check `Delete on Termination`
  * Step 5: add tag
    * ![Tag](../images/aws_ec2/tag.png)
    * Really useful for optimise cost, and to see where you cost coming from
    * You should tag everything, as mush as possible
  * Step 6: config security group
    * It's virtual firewall
    * ![Security](../images/aws_ec2/security.png)
  * Step 7: review
    * ![Review](../images/aws_ec2/review.png)
    * create key pair
      * public key: the padlock
      * private key: the key
      * ![Key Pair](../images/aws_ec2/key.png)
  * Launch Status
   * ![Launch](../images/aws_ec2/launch.png)
   * Status
     * ![Status](../images/aws_ec2/status.png)

* Access EC2 instance
  * change mode of private key: `chmod 400 MyEC2KeyPair.pem`
  * ssh into it: `ssh ec2-user@11.22.33.44 -i MyEC2KeyPair.pem`
  * change to root: `sudo su`
  * apply security update: `yum update -y`
  * install a httpd: `yum install httpd -y`
  * create a file in `/var/www/html/index.html` with content `<html><h1>Hello AWS!</h1></html>`
  * start htppd: `service httpd start`
  * make sure service start on reboot: `chkconfig httpd on`
  * You get:
    * ![Hello](../images/aws_ec2/hello.png)

* Status Checks Tab
  * ![Status checks](../images/aws_ec2/status_checks.png)
  * System Status Checks
    * ![System](../images/aws_ec2/system.png)
  * Instance tatus Checks
    * ![Instance](../images/aws_ec2/instance.png)  

* Monitoring Tab
  * Basic monitoring is every 5 mins
  * We can turn on detail monitoring, which is every 1 min, will cost a little bit extra
  * ![Monitoring](../images/aws_ec2/monitoring.png)

* Tips 1
  * ![Tip1](../images/aws_ec2/tip1.png)

* Security Group
  * A virtual firewall
  * rules apply immediately
  * security group is stateful
  * everything is blocked by default
  * ![Tip2](../images/aws_ec2/tip2.png)

* EBS, Volumes & Snapsots
  * ![Tip3](../images/aws_ec2/tip3.png)
  * -
  * ![Tip4](../images/aws_ec2/tip4.png)
  * -
  * ![Tip5](../images/aws_ec2/tip5.png)

* RAID, Volumes, Snapshots
  * Remember the Lab using Windows Server 2016 to Create a RAID0 Array
  * Question: How can I take a Snapshot of a RAID Array?
  * ![raid](../images/aws_ec2/raid.png)
    * Stop the application from wrting to disk
    * Flash all caches to the disk
    * How can we do this?
      * Freeze the file system
      * Unmount the RAID Array
      * Shutting down the associated EC2 instance.

* Encrypted Root Device Volumes & Snapshots
  * Stop instance for consistency
  * Create a snapshot
  * Copy snapshot to another Region
    * You can encryption this snapshot
  * Change region to find the newly copied snapshot
  * Create a image from this EBS snapshot
  * Use this image to boot a ec2 instance which root volume is encrypted
  * Tips
    * To create a snapshot for Amazon EBS volumes that serve as root devices, you should stop the instance before taking the snapshot
    * Snapshots of encrypted volumes are encrypted automatically
    * Volumes restored from encrypted snapshots are encrypted automatically
    * You can share snapshots, but only if they are unencrypted
      * these snapshots can be shared with other AWS accounts or made public

* AMI Types (EBS vs Instance Store)
  * You can select your AMI based on
    * ![ami type](../images/aws_ec2/ami_types.png)
      * instance store can't be stop, only terminate and restart, get less durability
  * EBS vs Instance Store
    * ![vs](../images/aws_ec2/ebs_vs_instance.png)
  * tips
    * ![ami tips](../images/aws_ec2/ebs_tips.png)


## Using Bootstrap Script

You can run your custom command during instance boot:

```
#!/bin/bash
yum install httpd -y
yum update -y
aws s3 cp s3://YOURBUCKETNAMEHERE/index.html /var/www/html/
service httpd start
chkconfig httpd on
```

## Get ec2 instance metadata

```
curl http://169.254.169.254/latest/meta-data/public-ipv4 # Give IPv4
curl http://169.254.169.254/latest/meta-data/user-data # Give you the bootstrap script
```

## Autoscaling

* Launch configuration
* Auto scaling groups

## EC2 placement group

* Cluster placement group
  * Big data
* Spread placement group
  * Two important instance in different AZ(distinct underlying hardware)
* ![Placement Group](../images/aws_ec2/placement_group.png)

## EFS(NFS)
