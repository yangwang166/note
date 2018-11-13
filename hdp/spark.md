# Spark General

## Install Spark on your mac

```bash
brew install apache-spark
```

Will be install in `/usr/local/Cellar/apache-spark/2.3.2/`

Then use

> `spark-shell`

A more colorful repl shell

> `spark-shell --conf spark.driver.extraJavaOptions="-Dscala.color"`

PySpark shell

> `pyspark`

## Install Spark on your ubuntu server

```bash
wget http://apache.mirror.amaze.com.au/spark/spark-2.3.2/spark-2.3.2-bin-hadoop2.7.tgz
tar zxvf spark-2.3.2-bin-hadoop2.7.tgz
cd spark-2.3.2-bin-hadoop2.7/bin
./spark-shell
```

## Run Spark on HDP cluster

Run Spark2

```bash
export SPARK_MAJOR_VERSION=2
spark-shell --conf spark.driver.extraJavaOptions="-Dscala.color"
```

## Spark shell context

`sc`: SparkContext

`spark.sqlContext`: SQL Context

## Scala load a string block

```scala
var json_file = s"""
     | {
     |   "project": "AU09-004",
     |   "datacode": 2200,
     |   "dataset": "Final PSTM Full Stack",
     |   "CDP X bytes": {
     |     "First": 73,
     |     "Last": 76
     |   },
     |   "polarity": "+IMP NEG"
     | }"""
```

## Load Binary File from HDFS

```scala
var folder_name = "/data2/AU09-004";
val segy_files = spark.sparkContext.binaryFiles(folder_name);
segy_files.map(_._1).collect().foreach(line => println(line));
/*
  Output:
   hdfs://path:8020/data2/AU09-004/AU09-004_2200_GA302-023.json
   hdfs://path:8020/data2/AU09-004/AU09-004_2200_GA302-023.sgy
*/
```

## Load Json

A sample Json

```json
{
    "project": "AU09-004",
    "datacode": 2200,
    "dataset": "Final PSTM Full Stack",
    "CDP X bytes": {
        "First": 73,
        "Last": 76
    },
    "polarity": "+IMP NEG"
}
```

```scala
import scala.util.parsing.json._;
var segy_json = JSON.parseFull(json_file)
var polarity = segy_json.get.asInstanceOf[Map[String, Any]]("polarity").asInstanceOf[String]
var cdp_x_first = segy_json.get.asInstanceOf[Map[String, Any]]("CDP X bytes").asInstanceOf[Map[String, Double]]("First")
```

## Tips

* In Spark shell, you can use `Tab` to know the attribute of current object
* Check the type of current object `:type your_object`

```bash
scala> :type json_file
String
```

* 