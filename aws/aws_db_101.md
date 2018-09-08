# AWS DB 101

## Some concepts

Relational database

* Database
  * Tables
  * Row
  * Fields (Columns)

* Products:
  * Oracle
  * Mysql
  * SQL server
  * Maria DB
  * PostgreSQL
  * Amazon Aurora

Non Relational Databases
* Database
  * Collection = Table
  * Document = Row
  * Key Value pairs = Fields(Columns)

Data Warehousing
* For BI
* Tools
  * Cognos
  * Jaspersoft
  * SQL Server Reporting services
  * Oracle Hyperion
  * SAP NetWeaver
* Used to pull in very large and complex data sets.
* Usually used by management to do queries on data (such as current performance vs target etc)
* Data Warehousing databases use different type of architecture both from a database perspective and infrastructure layer

OLTP vs OLAP
* OLTP: Online Transaction processing
  * example: order number 12345
    * Pull up a row of data such as Name, Date, Address to Deliver to, Deliver Status, etc  
* OLAP: Online Analytic processing (For Data Warehousing)
  * Example: Net profit for EMEA and Pacific for Digital Radio Product
    * Pull in large number of records
    * Sum of Radios Sold in EMEA
    * Sum of Radio Sold in Pacific
    * Unit Cost of Radio in each Region
    * Sales price for each radio
    * Sales price - Unit Cost

ElasticCache
* a Web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud. The service improves the performance of web application by allowing you to retrieve information from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases.
* ElasticCache support two open source in memory caching engines:
  * Memcached
  * Redis
* Relief your databases

## Summary

* RDS (OLTP)
  * SQL Server
  * MySQL
  * PostgresSQL
  * Oracle
  * Aurora
  * MariaDB
* DynamoDB (No SQL)
* RedShift (OLAP, Data warehousing)
* Elasticache (In Memory Caching)
