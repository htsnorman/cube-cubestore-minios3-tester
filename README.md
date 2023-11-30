
# Cubejs Cubestore MinioS3 tester

- ```docker-compose up -d```

- Create http://127.0.0.1:19001/ demoapp bucket
- (maybe docker break again ```docker-compose up -d```)

- ```mysql -uroot -h127.0.0.1```

- Run SQL

```
create schema demoapp;

create table demoapp.demoapp(id varchar(100),age int);

insert into demoapp.demoapp(id,age) values('1',3);

select * from demoapp.demoapp;
```

- Error appear
ERROR 1815 (HY000): File 1-2ksk75rn.chunk.parquet can't be listed after upload. Either there's Cube Store cluster misconfiguration, or storage can't provide the required consistency.

full output:

```
MySQL [(none)]> create schema demoapp;
+------+---------+
| id   | name    |
+------+---------+
|    1 | demoapp |
+------+---------+
1 row in set (0.001 sec)

MySQL [(none)]> create table demoapp.demoapp(id varchar(100),age int);
+------+------------+-----------+-------------------------------------------------------------------------------------------------------------+-----------+---------------+----------+----------+-----------------------------------+-----------------+---------+--------+------------------+----------------+---------------+---------------------------+--------------------------+------------------+-------------------------+---------------------------+
| id   | table_name | schema_id | columns                                                                                                     | locations | import_format | has_data | is_ready | created_at                        | build_range_end | seal_at | sealed | select_statement | source_columns | stream_offset | unique_key_column_indices | aggregate_column_indices | seq_column_index | location_download_sizes | partition_split_threshold |
+------+------------+-----------+-------------------------------------------------------------------------------------------------------------+-----------+---------------+----------+----------+-----------------------------------+-----------------+---------+--------+------------------+----------------+---------------+---------------------------+--------------------------+------------------+-------------------------+---------------------------+
|    1 | demoapp    | 1         | [{"name":"id","column_type":"String","column_index":0},{"name":"age","column_type":"Int","column_index":1}] | NULL      | NULL          | false    | true     | 2023-11-30 10:25:19.030885203 UTC | NULL            | NULL    | false  | NULL             | NULL           | NULL          | NULL                      |                          | NULL             | NULL                    | NULL                      |
+------+------------+-----------+-------------------------------------------------------------------------------------------------------------+-----------+---------------+----------+----------+-----------------------------------+-----------------+---------+--------+------------------+----------------+---------------+---------------------------+--------------------------+------------------+-------------------------+---------------------------+
1 row in set (0.001 sec)

MySQL [(none)]> insert into demoapp.demoapp(id,age) values('1',3);
ERROR 1815 (HY000): File 1-2ksk75rn.chunk.parquet can't be listed after upload. Either there's Cube Store cluster misconfiguration, or storage can't provide the required consistency.
MySQL [(none)]> select * from demoapp.demoapp;
Query OK, 0 rows affected (0.020 sec)
```



## IF define minio subpath error dont appear:
(commented line in compose)
- CUBESTORE_MINIO_SUB_PATH=cubesubpath

```
MySQL [(none)]> create schema demoapp;
+------+---------+
| id   | name    |
+------+---------+
|    1 | demoapp |
+------+---------+
1 row in set (0.001 sec)

MySQL [(none)]> create table demoapp.demoapp(id varchar(100),age int);
+------+------------+-----------+-------------------------------------------------------------------------------------------------------------+-----------+---------------+----------+----------+-----------------------------------+-----------------+---------+--------+------------------+----------------+---------------+---------------------------+--------------------------+------------------+-------------------------+---------------------------+
| id   | table_name | schema_id | columns                                                                                                     | locations | import_format | has_data | is_ready | created_at                        | build_range_end | seal_at | sealed | select_statement | source_columns | stream_offset | unique_key_column_indices | aggregate_column_indices | seq_column_index | location_download_sizes | partition_split_threshold |
+------+------------+-----------+-------------------------------------------------------------------------------------------------------------+-----------+---------------+----------+----------+-----------------------------------+-----------------+---------+--------+------------------+----------------+---------------+---------------------------+--------------------------+------------------+-------------------------+---------------------------+
|    1 | demoapp    | 1         | [{"name":"id","column_type":"String","column_index":0},{"name":"age","column_type":"Int","column_index":1}] | NULL      | NULL          | false    | true     | 2023-11-30 10:50:17.843622198 UTC | NULL            | NULL    | false  | NULL             | NULL           | NULL          | NULL                      |                          | NULL             | NULL                    | NULL                      |
+------+------------+-----------+-------------------------------------------------------------------------------------------------------------+-----------+---------------+----------+----------+-----------------------------------+-----------------+---------+--------+------------------+----------------+---------------+---------------------------+--------------------------+------------------+-------------------------+---------------------------+
1 row in set (0.001 sec)

MySQL [(none)]> insert into demoapp.demoapp(id,age) values('1',3);
Query OK, 0 rows affected (0.061 sec)

MySQL [(none)]> select * from demoapp.demoapp;
+------+------+
| id   | age  |
+------+------+
| 1    |    3 |
+------+------+
1 row in set (0.044 sec)

MySQL [(none)]>
```
