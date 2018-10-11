# Hive Related

## Beeline

* using Hive1

```
beeline -u 'jdbc:hive2://zk:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2'
```

* using Hive2 interactive

```
beeline -u 'jdbc:hive2://zk:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2-hive2'
```

## Issue:

> LLAP app 'llap0' in 'RUNNING_PARTIAL' state. Live Instances : '4'. Desired Instances : '6' after 409.551193953 secs.

Solution:

Increase: `Number of retries while checking LLAP app status`

Root cause: see `/var/lib/ambari-agent/cache/common-services/HIVE/0.12.0.2.0/package/scripts/hive_server_interactive.py`

``` python
def check_llap_app_status_in_llap_ga(self, llap_app_name, num_retries, return_immediately_if_stopped=False):
  curr_time = time.time()
  total_timeout = int(num_retries) * 20; # Total wait time while checking the status via llapstatus command
  Logger.debug("Calculated 'total_timeout' : {0} using config 'num_retries_for_checking_llap_status' : {1}".format(total_timeout, num_retries))
  refresh_rate = 2 # Frequency of checking the llapstatus
  percent_desired_instances_to_be_up = 80 # Out of 100.
  llap_app_info = self._get_llap_app_status_info_in_llap_ga(percent_desired_instances_to_be_up/100.0, total_timeout, refresh_rate)
```
