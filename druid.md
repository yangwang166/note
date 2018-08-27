# Druid notes


## Stop a task

>`curl -X POST http://<druid_coordinator>:8081/druid/indexer/v1/task/<task_id>/shutdown`

## Stop a supervisor

> `curl -X POST http://<druid_coordinator>:8081/druid/indexer/v1/supervisor/<supervisorId>/shutdown`

## Get supervisor list
> `curl http://<druid_coordinator>:8081/druid/indexer/v1/supervisor`



## Ref
[Kafka Indexing Service](http://druid.io/docs/latest/development/extensions-core/kafka-ingestion.html)
