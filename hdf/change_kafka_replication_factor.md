# Change Kafka Replication Factor

Find broker list, we need the broker `ids` in the following steps

> `./zookeeper-shell.sh zk_host:2181 <<< "ls /brokers/ids"`

For example, we have a topic, which has 2 partitions, and replica number is 2

> `bin/kafka-topics.sh --zookeeper zk1:2181,zk2:2181,zk3:2181/kafka --create --replication-factor 2 --partitions 2 --topic testTopic1`

Check the topics

> `bin/kafka-topics.sh --describe --zookeeper zk1:2181,zk2:2181,zk3:2181/kafka --topic testTopic1`

We got

```bash
Topic:testTopic1    PartitionCount:2    ReplicationFactor:2    Configs:
    Topic: testTopic1    Partition: 0    Leader: 1    Replicas: 1,2    Isr: 1,2
    Topic: testTopic1    Partition: 1    Leader: 2    Replicas: 2,3    Isr: 2,3
```

Isr is `in-sync replicas`

We know the leader of partition 0 is broker with id=`1`

We know the leader of partition 1 is broker with id=`2`

We produce some data in to topic

> `bin/kafka-console-producer.sh --broker-list broker:9092,broker2:9092,broker3:9092 --topic testTopic1 < data.file`

Then check the data in the kafka log folder


> `du -sh /opt/kafka/kafka-logs/testTopic*`

We got

```
data01: 44K    /opt/kafka/kafka-logs/testTopic1-0
data02: 44K    /opt/kafka/kafka-logs/testTopic1-0
data02: 4.0K    /opt/kafka/kafka-logs/testTopic1-1
data03: 4.0K    /opt/kafka/kafka-logs/testTopic1-1
```

## Now we begin to change the replication factor

We define a json file to describe how we change the replication factor. Here we change the original replication factor from 2 to 3

`replication.json`

``` json
{
    "version": 1,
    "partitions": [
        {
            "topic": "testTopic1",
            "partition": 0,
            "replicas": [
                1,
                2,
                3
            ]
        },
        {
            "topic": "testTopic1",
            "partition": 1,
            "replicas": [
                2,
                3,
                1
            ]
        }
    ]
}
```

Execute the change

> `bin/kafka-reassign-partitions.sh --zookeeper zk1:2181,zk2:2181,zk3:2181/kafka --reassignment-json-file replication.json --execute`

This may run for some time, we can use following command to check status

> `bin/kafka-reassign-partitions.sh --zookeeper zk1:2181,zk2:2181,zk3:2181/kafka --reassignment-json-file replication.json --verify`

Check the topic

> `bin/kafka-topics.sh --describe --zookeeper data01:2181,data02:2181,data03:2181/kafka --topic testTopic1`

We got:

```bash
Topic:testTopic1    PartitionCount:2    ReplicationFactor:3    Configs:
    Topic: testTopic1    Partition: 0    Leader: 1    Replicas: 1,2,3    Isr: 1,2,3
    Topic: testTopic1    Partition: 1    Leader: 2    Replicas: 2,3,1    Isr: 2,3,1
```

And check the files in the kafka log folder:

> `du -sh /opt/kafka/kafka-logs/testTopic*`

```
data01: 44K    /opt/kafka/kafka-logs/testTopic1-0
data01: 4.0K    /opt/kafka/kafka-logs/testTopic1-1
data03: 40K    /opt/kafka/kafka-logs/testTopic1-0
data03: 4.0K    /opt/kafka/kafka-logs/testTopic1-1
data02: 44K    /opt/kafka/kafka-logs/testTopic1-0
data02: 4.0K    /opt/kafka/kafka-logs/testTopic1-1
```

## Complementary

kafka-reassign-partitions.sh also have an para `--generate`, it can give reassign suggestion.
