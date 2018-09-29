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
