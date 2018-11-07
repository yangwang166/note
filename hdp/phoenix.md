# Phoenix General

## Login into client

```
cd /usr/hdp/current/phoenix-client/bin
./sqlline.py zk1:2181;zk2:2181;zk3:2181
```

## sqlline Commends

Show help

> `help`

Show table list

> `!tables`

## sql

Create table

```sql
CREATE TABLE TABLE1 (
  ID BIGINT NOT NULL PRIMARY KEY,
  COL1 VARCHAR
);
```

Upsert into table

```sql
UPSERT INTO TABLE1 (ID, COL1) VALUES (1, 'test_row_1');
UPSERT INTO TABLE1 (ID, COL1) VALUES (2, 'test_row_2');
```

Select from table

```sql
select * from table1;
```

Or

```sql
!sql select * from table1;
```
