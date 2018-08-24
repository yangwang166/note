# Uninstall HDP and Ambari

```bash
#cd /usr/lib/python2.6/site-packages/ambari_agent/
#python HostCleanup.py --skip="users"

#yum remove hive*
#yum remove oozie*
#yum remove pig*
#yum remove zookeeper*
#yum remove tez*
#yum remove hbase*
#yum remove ranger*
#yum remove knox*
#yum remove storm*
#yum remove accumulo*
#yum remove falcon*
#yum remove ambari-metrics-hadoop-sink
#yum remove smartsense-hst
#yum remove slider*
#yum remove ambari-metrics-monitor
#yum remove spark2*
#yum remove spark*
#yum remove ambari-infra-solr-client

rm -rf /var/log/ambari-agent
rm -rf /var/log/ambari-metrics-grafana
rm -rf /var/log/ambari-metrics-monitor
rm -rf /var/log/ambari-server/
rm -rf /var/log/falcon
rm -rf /var/log/flume
rm -rf /var/log/hadoop
rm -rf /var/log/hadoop-mapreduce
rm -rf /var/log/hadoop-yarn
rm -rf /var/log/hive
rm -rf /var/log/hive-hcatalog
rm -rf /var/log/hive2
rm -rf /var/log/hst
rm -rf /var/log/knox
rm -rf /var/log/oozie
rm -rf /var/log/solr
rm -rf /var/log/zookeeper
rm -rf /var/log/smartsense-activity

rm -rf /hadoop
rm -rf /hdp/*
rm -rf /zookeeper/*
rm -rf /kafka/*
rm -rf /usr/bin/hadoop
rm -rf /usr/hdp/*

rm -rf /etc/ambari-agent
rm -rf /etc/ambari-metrics-grafana
rm -rf /etc/ambari-server
rm -rf /etc/ams-hbase
rm -rf /etc/falcon
rm -rf /etc/flume
rm -rf /etc/hadoop
rm -rf /etc/hadoop-httpfs
rm -rf /etc/hbase
rm -rf /etc/hive
rm -rf /etc/hive-hcatalog
rm -rf /etc/hive-webhcat
rm -rf /etc/hive2
rm -rf /etc/hst
rm -rf /etc/knox
rm -rf /etc/livy
rm -rf /etc/mahout
rm -rf /etc/oozie
rm -rf /etc/phoenix
rm -rf /etc/pig
rm -rf /etc/ranger-admin
rm -rf /etc/ranger-usersync
rm -rf /etc/spark2
rm -rf /etc/tez
rm -rf /etc/tez_hive2
rm -rf /etc/zookeeper

rm -rf /var/run/ambari-agent
rm -rf /var/run/ambari-metrics-grafana
rm -rf /var/run/ambari-server
rm -rf /var/run/falcon
rm -rf /var/run/flume
rm -rf /var/run/hadoop
rm -rf /var/run/hadoop-mapreduce
rm -rf /var/run/hadoop-yarn
rm -rf /var/run/hbase
rm -rf /var/run/hive
rm -rf /var/run/hive-hcatalog
rm -rf /var/run/hive2
rm -rf /var/run/hst
rm -rf /var/run/knox
rm -rf /var/run/oozie
rm -rf /var/run/webhcat
rm -rf /var/run/zookeeper
rm -rf /var/run/smartsense-activity-analyzer
rm -rf /var/run/smartsense-activity-explorer


rm -rf /usr/lib/ambari-agent
rm -rf /usr/lib/ambari-infra-solr-client
rm -rf /usr/lib/ambari-metrics-hadoop-sink
rm -rf /usr/lib/ambari-metrics-kafka-sink
rm -rf /usr/lib/ambari-server-backups
rm -rf /usr/lib/ams-hbase
rm -rf /usr/lib/mysql
rm -rf /var/lib/ambari-agent
rm -rf /var/lib/ambari-metrics-grafana
rm -rf /var/lib/ambari-server
rm -rf /var/lib/flume
rm -rf /var/lib/hadoop-hdfs
rm -rf /var/lib/hadoop-mapreduce
rm -rf /var/lib/hadoop-yarn
rm -rf /var/lib/hive2
rm -rf /var/lib/knox
rm -rf /var/lib/smartsense
rm -rf /var/lib/storm

rm -rf /var/tmp/*

#remove crontab
#0 * * * * /usr/hdp/share/hst/bin/hst-scheduled-capture.sh sync
#0 2 * * 0 /usr/hdp/share/hst/bin/hst-scheduled-capture.sh

#yum remove mysql mysql-server
#yum remove postgresql
rm -rf /var/lib/pgsql
rm -rf /var/lib/mysql

cd /usr/bin
rm -rf accumulo
rm -rf atlas-start
rm -rf atlas-stop
rm -rf beeline
rm -rf falcon
rm -rf flume-ng
rm -rf hbase
rm -rf hcat
rm -rf hdfs
rm -rf hive
rm -rf hiveserver2
rm -rf kafka
rm -rf mahout
rm -rf mapred
rm -rf oozie
rm -rf oozied.sh
rm -rf phoenix-psql
rm -rf phoenix-queryserver
rm -rf phoenix-sqlline
rm -rf phoenix-sqlline-thin
rm -rf pig
rm -rf python-wrap
rm -rf ranger-admin
rm -rf ranger-admin-start
rm -rf ranger-admin-stop
rm -rf ranger-kms
rm -rf ranger-usersync
rm -rf ranger-usersync-start
rm -rf ranger-usersync-stop
rm -rf slider
rm -rf sqoop
rm -rf sqoop-codegen
rm -rf sqoop-create-hive-table
rm -rf sqoop-eval
rm -rf sqoop-export
rm -rf sqoop-help
rm -rf sqoop-import
rm -rf sqoop-import-all-tables
rm -rf sqoop-job
rm -rf sqoop-list-databases
rm -rf sqoop-list-tables
rm -rf sqoop-merge
rm -rf sqoop-metastore
rm -rf sqoop-version
rm -rf storm
rm -rf storm-slider
rm -rf worker-lanucher
rm -rf yarn
rm -rf zookeeper-client
rm -rf zookeeper-server
rm -rf zookeeper-server-cleanup

userdel -r accumulo
userdel -r ambari-qa
userdel -r ams
userdel -r falcon
userdel -r flume
userdel -r hbase
userdel -r hcat
userdel -r hdfs
userdel -r hive
userdel -r kafka
userdel -r knox
userdel -r mapred
userdel -r oozie
userdel -r ranger
userdel -r spark
userdel -r sqoop
userdel -r storm
userdel -r tez
userdel -r yarn
userdel -r zeppelin
userdel -r zookeeper
userdel -r ambari
userdel -r postgres
userdel -r activity_analyzer

groupdel postgres
groupdel hadoop
groupdel zookeeper
groupdel ambari
groupdel hdfs
groupdel yarn
groupdel mapred
```

Double check with

```bash
find / -name *ambari*
find / -name *accumulo*
find / -name *atlas*
find / -name *beeline*
find / -name *falcon*
find / -name *flume*
find / -name *hadoop*
find / -name *hbase*
find / -name *hcat*
find / -name *hdfs*
find / -name *hdp*
find / -name *hive*
find / -name *hiveserver2*
find / -name *kafka*
find / -name *mahout*
find / -name *mapred*
find / -name *oozie*
find / -name *phoenix*
find / -name *pig*
find / -name *ranger*
find / -name *slider*
find / -name *sqoop*
find / -name *storm*
find / -name *yarn*
find / -name *zookeeper*
```

ref: https://community.hortonworks.com/articles/97489/completely-uninstall-hdp-and-ambari.html
