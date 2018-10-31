# Provision 8 nodes HDP3 cluster with Ansible on OpenStack

## 1 Based on `https://github.com/yangwang166/hwx_ansible`

## 2 Add ssh password free access to all nodes


## 3 Modify `inventory/static`

```
[hdp-management]
management ansible_host=searcher-a.field.hortonworks.com ansible_user=centos ansible_ssh_private_key_file="~/.ssh/id_rsa" rack=/default-rack

[hdp-master-1]
master1 ansible_host=searcher-m1.field.hortonworks.com ansible_user=centos ansible_ssh_private_key_file="~/.ssh/id_rsa" rack=/default-rack

[hdp-master-2]
master2 ansible_host=searcher-m2.field.hortonworks.com ansible_user=centos ansible_ssh_private_key_file="~/.ssh/id_rsa" rack=/default-rack

[hdp-master-3]
master3 ansible_host=searcher-m3.field.hortonworks.com ansible_user=centos ansible_ssh_private_key_file="~/.ssh/id_rsa" rack=/default-rack

[hdp-worker]
data1 ansible_host=searcher-d1.field.hortonworks.com ansible_user=centos ansible_ssh_private_key_file="~/.ssh/id_rsa" rack=/default-rack
data2 ansible_host=searcher-d2.field.hortonworks.com ansible_user=centos ansible_ssh_private_key_file="~/.ssh/id_rsa" rack=/default-rack
data3 ansible_host=searcher-d3.field.hortonworks.com ansible_user=centos ansible_ssh_private_key_file="~/.ssh/id_rsa" rack=/default-rack

[hdp-edge]
edge ansible_host=searcher-e.field.hortonworks.com ansible_user=centos ansible_ssh_private_key_file="~/.ssh/id_rsa" rack=/default-rack
```

## 4 Modify `playbooks/group_vars/all`

Change the blueprint part as bellow

```yaml
#############################
## blueprint configuration ##
#############################

blueprint_name: '{{ cluster_name }}_blueprint'                  # the name of the blueprint as it will be stored in Ambari
blueprint_file: 'blueprint_dynamic.j2'                          # the blueprint JSON file - 'blueprint_dynamic.j2' is a Jinja2 template that generates the required JSON
blueprint_dynamic:                                              # properties for the dynamic blueprint - these are only used by the 'blueprint_dynamic.j2' template to generate the JSON
  - host_group: "hdp-management"
    clients: ['ZOOKEEPER_CLIENT', 'HDFS_CLIENT', 'YARN_CLIENT', 'MAPREDUCE2_CLIENT', 'TEZ_CLIENT', 'PIG', 'SQOOP', 'HIVE_CLIENT', 'OOZIE_CLIENT', 'INFRA_SOLR_CLIENT', 'SPARK2_CLIENT', 'HBASE_CLIENT']
    services:
      - AMBARI_SERVER
      - INFRA_SOLR
      - LOGSEARCH_SERVER
      - OOZIE_SERVER
      - HST_SERVER
      - ACTIVITY_ANALYZER
      - ACTIVITY_EXPLORER
      - HST_AGENT
      - METRICS_COLLECTOR
      - METRICS_GRAFANA
      - METRICS_MONITOR
      - RANGER_ADMIN
      - RANGER_USERSYNC
  - host_group: "hdp-master-1"
    clients: ['ZOOKEEPER_CLIENT', 'HDFS_CLIENT', 'YARN_CLIENT', 'MAPREDUCE2_CLIENT', 'TEZ_CLIENT', 'PIG', 'SQOOP', 'HIVE_CLIENT', 'OOZIE_CLIENT', 'INFRA_SOLR_CLIENT', 'SPARK2_CLIENT', 'HBASE_CLIENT']
    services:
      - ZOOKEEPER_SERVER
      - NAMENODE
      - JOURNALNODE
      - ZKFC
      - RESOURCEMANAGER
      - HIVE_SERVER
      - HIVE_METASTORE
      - HST_AGENT
      - METRICS_MONITOR
      - LOGSEARCH_LOGFEEDER
  - host_group: "hdp-master-2"
    clients: ['ZOOKEEPER_CLIENT', 'HDFS_CLIENT', 'YARN_CLIENT', 'MAPREDUCE2_CLIENT', 'TEZ_CLIENT', 'PIG', 'SQOOP', 'HIVE_CLIENT', 'OOZIE_CLIENT', 'INFRA_SOLR_CLIENT', 'SPARK2_CLIENT', 'HBASE_CLIENT']
    services:
      - ZOOKEEPER_SERVER
      - NAMENODE
      - ZKFC
      - JOURNALNODE
      - RESOURCEMANAGER
      - HBASE_MASTER
      - HIVE_SERVER
      - HIVE_METASTORE
      - HST_AGENT
      - METRICS_MONITOR
      - YARN_REGISTRY_DNS
      - TIMELINE_READER
      - LOGSEARCH_LOGFEEDER
  - host_group: "hdp-master-3"
    clients: ['ZOOKEEPER_CLIENT', 'HDFS_CLIENT', 'YARN_CLIENT', 'MAPREDUCE2_CLIENT', 'TEZ_CLIENT', 'PIG', 'SQOOP', 'HIVE_CLIENT', 'OOZIE_CLIENT', 'INFRA_SOLR_CLIENT', 'SPARK2_CLIENT', 'HBASE_CLIENT']
    services:
      - ZOOKEEPER_SERVER
      - NAMENODE
      - ZKFC
      - JOURNALNODE
      - HBASE_MASTER
      - HST_AGENT
      - METRICS_MONITOR
      - APP_TIMELINE_SERVER
      - HISTORYSERVER
      - SPARK2_JOBHISTORYSERVER
      - LOGSEARCH_LOGFEEDER
  - host_group: "hdp-worker"
    clients: ['ZOOKEEPER_CLIENT', 'HDFS_CLIENT', 'YARN_CLIENT', 'MAPREDUCE2_CLIENT', 'TEZ_CLIENT', 'PIG', 'SQOOP', 'HIVE_CLIENT', 'OOZIE_CLIENT', 'INFRA_SOLR_CLIENT', 'SPARK2_CLIENT', 'HBASE_CLIENT']
    services:
      - DATANODE
      - NODEMANAGER
      - HBASE_REGIONSERVER
      - HST_AGENT
      - METRICS_MONITOR
      - LOGSEARCH_LOGFEEDER
  - host_group: "hdp-edge"
    clients: ['ZOOKEEPER_CLIENT', 'HDFS_CLIENT', 'YARN_CLIENT', 'MAPREDUCE2_CLIENT', 'TEZ_CLIENT', 'PIG', 'SQOOP', 'HIVE_CLIENT', 'OOZIE_CLIENT', 'INFRA_SOLR_CLIENT', 'SPARK2_CLIENT', 'HBASE_CLIENT']
    services:
      - ZEPPELIN_MASTER
      - KNOX_GATEWAY
      - HST_AGENT
      - METRICS_MONITOR
      - LOGSEARCH_LOGFEEDER
```

## 5 Test the nodes

```
export CLOUD_TO_USE=static
source ~/ansible2/bin/activate
cd /Users/ywang/git/ansible-hortonworks
ansible -i inventory/static all --list-hosts
ansible -i inventory/static all -m setup
```

## 6 Prepare nodes

```
./prepare_nodes.sh
```

* Will apply `common`, `databases`, `krb5-client` and `mit-kdc` roles
* And installs required OS packages (including java)
* Apply the recommended OS settings
* Prepares databases
* Install local mit-kdc

### Detail

* Add nodes to hadoop-cluster group
* Add nodes to the ambari-server group
* Populate the namenode groups list
* Populate the ZKFailoverController groups list
* Populate the resourcemanager groups list
* Populate the journalnode groups list
* Populate the hive_mysql_embedded groups list
* Populate the zookeeper groups list
* Populate the zookeeper hosts list
* Populate the hiveserver hosts list
* Populate the oozie hosts list
* Populate the rangeradmin groups list
* Populate the rangeradmin hosts list
* Populate the all services and clients lists
* Set the install_hdp variable
* Check if /usr/bin/python exists
* Set the install_hdp variable to all nodes
* Gathering Facts
* common : Load variables
* common : Install required packages
* common : Install OpenJDK
* common : Make sure the NTP service is started
* common : Make sure the NTP service is enabled
* common : Make sure /etc/security/limits.d exists
* common : Set nofile and nproc limits
* common : Set swappiness to 1
* common : Stop the firewall service
* common : Disable the firewall service
* common : Disable selinux
* common : Disable Transparent Huge Pages until the next reboot
* common : Disable Transparent Huge Pages in Grub 2
* common : Generate the Grub config file
* common : Create tuned directory
* common : Upload the tuned profile
* common : Activate the tuned profile
* common : Attempt to fetch the http secret key
* common : Create the http secret key locally
* common : Copy the local http secret key to all nodes
* common : Delete the local http secret key
* krb5-client : Load variables
* krb5-client : Install the Kerberos client package
* krb5-client : Upload the krb5.conf file
* mit-kdc : Install MIT KDC packages
* mit-kdc : Upload the KDC configuration file
* mit-kdc : Create the KDC database for realm SEARCHER.COM
* mit-kdc : Restart krb5
* mit-kdc : Restart kadmin
* mit-kdc : Add the admin principal for realm SEARCHER.COM
* mit-kdc : Set the ACL for the admin user
* mit-kdc : Restart kadmin
* mit-kdc : Make sure the kdc service is started
* mit-kdc : Make sure the kdc service is enabled
* mit-kdc : Make sure the kadmin service is started
* mit-kdc : Make sure the kadmin service is enabled

## 7 Install Ambari

```
./install_ambari.sh
```

* Will apply `ambari-agent` and `ambari-server` roles
* Add ambari repo
* Install Ambari Agent, Server packages
* Configure Ambari server with the required java and databases options
* The result of this stage is an Ambari install ready to deploy a cluster

### Detail

* ambari-repo : Load variables
* ambari-repo : Add the Ambari repository (yum)
* ambari-agent : Install the ambari-agent package
* ambari-agent : Set the Ambari Server in the agent configuration
* ambari-agent : Configure the Ambari Agents to use TLS 1.2
* ambari-agent : Reload systemd
* ambari-agent : Restart ambari-agent
* ambari-agent : Make sure the ambari-agent service is started
* ambari-agent : Make sure the ambari-agent service is enabled
* ambari-server : Install the ambari-server package
* ambari-server : Reload systemd
* ambari-server : Install mysql required packages (for hive embedded)
* ambari-server : Configure the Ambari JDBC driver for mysql (for hive embedded)
* ambari-server : Set the Ambari Server Java setup option (OpenJDK)
* ambari-server : Run Ambari Server setup
* ambari-server : Increase the Ambari Server startup timeout
* ambari-server : Restart ambari-server
* ambari-server : Wait for Ambari Server to start listening on port 8080
* ambari-server : Make sure the ambari-server service is started
* ambari-server : Make sure the ambari-server service is enabled


## 8 Configure Ambari

```
./configure_ambari.sh
```

* apply `ambari-config` role
* further configures ambari with some settings
* Change admin password
* adds the repository information needed by the build

### Detail
