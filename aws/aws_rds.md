# AWS RDS

* There is a internal security group inside RDS, need to set it to allow RDS instance to talk to EC2, eg.

## Back ups, Multi-AZ, Read Replicas

* Automated Backups
  * retention period can be between 1 to 35 days
  * take full daily snapshots, and will store transaction logs throughout the day
  * point in time recovery to a second
  * when do recovery, aws will first choose the most recent daily backup, and then apply transaction logs relevant to that day.
  * enabled by default
  * backup data is stored in S3
  * get free storage space to the size of your db
  * Backup are taken within a defined window.
  * During the backup window, storage I/O may be suspended while you data is being backed up and you may experience elevated latency.
* Database Snapshots
  * done manually
  * they are even stored even after you delete the original RDS instance
* Restoring backup
  * whenever you restore either an automated backup or a manually snapshot, the restored version of the database will be a new RDS instance with a new DNS endpoint
  * original.region.rds.amazonaws.com -> restored.region.rds.amazonaws.com
* Multi-AZ
  * For disaster recover only, not performance improvement
  * always use dns endpoint, which amazon will deal with failover without administrative intervention
  * downtime can be within minutes
  * Aurora by default has builtin multi-AZ
* Read Replicas, for scaling
  * Available for
    * Mysql
    * PostgreSQL
    * MariaDB
    * Aurora
  * must have auto backup turned on in order to deploy a read replica
  * have up to 5 replica copies of any databases
  * you can have read replica of read replica (but watch out for latency)
  * each read replica will have it's own DNS endpoint
  * you can have read replica that have multi AZ
  * you can create read replica of multi AZ source databases
  * Read replica can be promoted to be their own databases, it will break the replication
  * you can have read replica in another region
  
