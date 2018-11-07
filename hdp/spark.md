# Spark General

## Install Spark on your mac

```
brew install apache-spark
```

Will be install in `/usr/local/Cellar/apache-spark/2.3.2/`

Then use

> `spark-shell`

> `pyspark`

## Install Spark on your ubuntu server

```
wget http://apache.mirror.amaze.com.au/spark/spark-2.3.2/spark-2.3.2-bin-hadoop2.7.tgz
tar zxvf spark-2.3.2-bin-hadoop2.7.tgz
cd spark-2.3.2-bin-hadoop2.7/bin
./spark-shell
```

## Run Spark on HDP cluster


Run Spark2

```bash
export SPARK_MAJOR_VERSION=2
spark-shell
```
