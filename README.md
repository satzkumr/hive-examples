
# Hive Examples

Creating External tables in Hive

1/ Create necessary Data file in HDFS

Sample Data

```
id,first_name,last_name,email,gender,ip_address
1,Tomlin,Berthome,tberthome0@a8.net,Polygender,33.14.127.145
2,Rasia,Fidele,rfidele1@state.gov,Female,216.129.65.214
3,Gaynor,Flintoffe,gflintoffe2@fda.gov,Female,173.80.121.114
4,Fernando,Freddi,ffreddi3@go.com,Male,245.130.44.60
5,Noellyn,Wolters,nwolters4@amazon.co.uk,Female,85.142.225.24
```
2/ Hive Beeline execute Query to create tables

```
CREATE EXTERNAL TABLE person_access (id int,first_name string,last_name string,email string,gender string,ip_address string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' STORED AS TEXTFILE location '/tmp/person_access';
```

3/ Resultant table would be something like this

```
+-------------------+---------------------------+--------------------------+-------------------------+-----------------------+---------------------------+
| person_access.id  | person_access.first_name  | person_access.last_name  |   person_access.email   | person_access.gender  | person_access.ip_address  |
+-------------------+---------------------------+--------------------------+-------------------------+-----------------------+---------------------------+
| 1                 | Tomlin                    | Berthome                 | tberthome0@a8.net       | Polygender            | 33.14.127.145             |
| 2                 | Rasia                     | Fidele                   | rfidele1@state.gov      | Female                | 216.129.65.214            |
| 3                 | Gaynor                    | Flintoffe                | gflintoffe2@fda.gov     | Female                | 173.80.121.114            |
| 4                 | Fernando                  | Freddi                   | ffreddi3@go.com         | Male                  | 245.130.44.60             |
| 5                 | Noellyn                   | Wolters                  | nwolters4@amazon.co.uk  | Female                | 85.142.225.24             |
+-------------------+---------------------------+--------------------------+-------------------------+-----------------------+---------------------------+
```

## Create ORC Table from above created table

```
CREATE TABLE person_access_orc STORED AS ORC AS SELECT * FROM person_access;
```

## Describe the table

```
DESCRIBE FORMATTED person_access_orc;
```

This should have the Storage format org.apache.hadoop.hive.ql.io.orc.OrcSerde and Input output Format with org.apache.hadoop.hive.ql.io.orc.OrcInputFormat org.apache.hadoop.hive.ql.io.orc.OrcOutputFormat

## Creating Parquet Strorage format table from the same table

```
CREATE TABLE person_access_parquet STORED AS PARQUET AS SELECT * FROM person_access;
```

Describe the table

This should have the Storage format  org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe along with Parquet input output format

## Create partitioned table

```
CREATE EXTERNAL TABLE account_trans (txnid int,name string,trans_amount string,ip_address string) PARTITIONED BY (trans_date string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',';
```

## Load data into the table

```
LOAD DATA INPATH '/tmp/transactions1.csv' INTO TABLE account_trans;
```

## Create another table with same above table with ORC format

```
CREATE EXTERNAL TABLE account_trans_orc LIKE account_trans STORED AS ORC;
```

## Load data into the table

```
INSERT OVERWRITE TABLE account_trans_orc PARTITION(trans_date) SELECT * FROM account_trans;
```

## Create another table same as previous table with PARQUET Format

```
CREATE EXTERNAL TABLE account_trans_parquet LIKE account_trans STORED AS PARQUET;
```
```
INSERT OVERWRITE TABLE account_trans_parquet PARTITION(trans_date) SELECT * FROM account_trans;
```


