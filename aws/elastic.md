# AWS managed ElasticSearch

Using curl to put doc into ES

```
Using curl to put data into ELK
* time curl -XPUT https://es_domain.us-east-1.es.amazonaws.com/dahling/gps/3 -d '{"Time": 1536649417.788, "Name": "TruckXX", "IP": "xx.xx.xx.xx", "UTC": "2018-09-11T07:03:16.400", "X": 300405.775, "Y": 334307.837, "Z": 511.221}' -H 'Content-Type: application/json'
```

Using Python to put doc into ES

```python
from elasticsearch import Elasticsearch, RequestsHttpConnection
from requests_aws4auth import AWS4Auth
import boto3

host = 'es_domain.us-east-1.es.amazonaws.com' # For example, my-test-domain.us-east-1.es.amazonaws.com
region = 'us-east-1'

service = 'es'
credentials = boto3.Session().get_credentials()

#awsauth = AWS4Auth(credentials.access_key, credentials.secret_key, region, service)

es = Elasticsearch(
    hosts = [{'host': host, 'port': 443}],
    use_ssl = True,
    #http_auth = awsauth,
    verify_certs = True,
    connection_class = RequestsHttpConnection
)

document = {
    "t": 1536649417.788,
    "a": "DT5120",
    "ip": "10.114.11.22",
    "UTC": "2018-09-12T07:03:16.400",
    "SVs": 15,
    "Flags1": "10111111",
    "Flags2": "00000001",
    "Easting": 300405.775,
    "Northing": 334307.837,
    "Height": 511.221
}


es.index(index="dahling", doc_type="gps", id="4", body=document)

print("Get document: ", es.get(index="dahling", doc_type="gps", id="4"))
```

Ref: https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-indexing-programmatic.html


## Use Elastic as TSDB


* https://logz.io/blog/influxdb-vs-elasticsearch/
  * The storage of time series data begins with defining mappings. In Elasticsearch, this means telling the engine how it should store the data, and also the fields that we are going to send for indexing. Defining this in advance improves the subsequent performance when querying the data.

* https://www.elastic.co/blog/elasticsearch-as-a-time-series-data-store
  * The most important part to start is your mapping. Defining your mapping ahead of time means that the analysis and storage of data in Elasticsearch is as optimal as possible.
  * metrics based use case
  * disabled \_source and \_all
  * building aggregations
  * save on disk space as the document stored will be smaller
  * downside:
    * won't be able to see the actual JSON documents
    * won't be able reindex to a new mapping or index structure

* Define Mapping:
  * A mapping defines the fields within a type, the datatype for each field, and how the field should be handled by Elasticsearch. A mapping is also used to configure metadata associated with the type.
  * Without mapping definition, elastic will use the default mapping, no schema
    * `curl -XPUT http://localhost:9200/test/item/1 -d '{"name":"zach", "description": "A Pretty cool guy."}'`
    * ES will auto use this mapping(schema):

``` bash
mappings: {
    item: {
        properties: {
            description: {
                type: string
            }
            name: {
                type: string
            }
        }
    }
}
```

* An example mapping definition:

``` bash
curl -XPUT 'http://localhost:9200/megacorp' -d '
{
    "settings": {
        "number_of_shards": 3,
        "number_of_replicas": 1
    },
    "mappings": {
        "employee": {
            "properties": {
                "first_name": {
                    "type": "string"
                },
                "last_name": {
                    "type": "string"
                },
                "age": {
                    "type": "integer"
                },
                "about": {
                    "type": "string"
                },
                "interests": {
                    "type": "string"
                },
                "join_time": {
                    "type": "date",
                    "format": "dateOptionalTime",
                    "index": "not_analyzed"
                }
            }
        }
    }
}
'
```

A metrics use case mapping (time series):

```
{
  "template": "stagemonitor-metrics-*",
  "settings": {
    "index": {
      "refresh_interval": "5s"
    }
  },
  "mappings": {
    "_default_": {
      "dynamic_templates": [
        {
          "strings": {
            "match": "*",
            "match_mapping_type": "string",
            "mapping":   { "type": "string",  "doc_values": true, "index": "not_analyzed" }
          }
        }
      ],
      "_all":            { "enabled": false },
      "_source":         { "enabled": false },
      "properties": {
        "@timestamp":    { "type": "date",    "doc_values": true },
        "count":         { "type": "integer", "doc_values": true, "index": "no" },
        "m1_rate":       { "type": "float",   "doc_values": true, "index": "no" },
        "m5_rate":       { "type": "float",   "doc_values": true, "index": "no" },
        "m15_rate":      { "type": "float",   "doc_values": true, "index": "no" },
        "max":           { "type": "integer", "doc_values": true, "index": "no" },
        "mean":          { "type": "integer", "doc_values": true, "index": "no" },
        "mean_rate":     { "type": "float",   "doc_values": true, "index": "no" },
        "median":        { "type": "float",   "doc_values": true, "index": "no" },
        "min":           { "type": "float",   "doc_values": true, "index": "no" },
        "p25":           { "type": "float",   "doc_values": true, "index": "no" },
        "p75":           { "type": "float",   "doc_values": true, "index": "no" },
        "p95":           { "type": "float",   "doc_values": true, "index": "no" },
        "p98":           { "type": "float",   "doc_values": true, "index": "no" },
        "p99":           { "type": "float",   "doc_values": true, "index": "no" },
        "p999":          { "type": "float",   "doc_values": true, "index": "no" },
        "std":           { "type": "float",   "doc_values": true, "index": "no" },
        "value":         { "type": "float",   "doc_values": true, "index": "no" },
        "value_boolean": { "type": "boolean", "doc_values": true, "index": "no" },
        "value_string":  { "type": "string",  "doc_values": true, "index": "no" }
      }
    }
  }
}
```

* Supported type
  * string
  * byte, short, integer, long
  * float, double
  * boolean
  * date

* index type:
  * analyzed: First analyze the string, then index it. In other words, index this field as full text.
  * not_analyzed: Index this field, so it is searchable, but index the value exactly as specified. Do not analyze it.
  * no: Don’t index this field at all. This field will not be searchable.


## Benchmarking
    * https://www.influxdata.com/blog/influxdb-markedly-elasticsearch-in-time-series-data-metrics-benchmark/

*
