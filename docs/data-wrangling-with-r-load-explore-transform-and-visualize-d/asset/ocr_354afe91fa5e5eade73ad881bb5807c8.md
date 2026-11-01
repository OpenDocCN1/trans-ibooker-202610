Figure 7.6: Sample data file open in Excel to understand the column structure and data types

2. Now, we will create a DDL statement and execute that in Athena to create the table:

![](bbox=[0.12, 0.14, 0.88, 0.47])

Figure 7.7: AWS Athena console – Creating a table manually using a DDL statement

# NOTE

The DML statement used is shown next. The DML statement was taken from a different Glue crawler to save time and changed to meet our needs here.

```
CREATE EXTERNAL TABLE `noaa_data_csv` ( `id` string,
  `date` bigint,
  `element` string,
  `data_value` bigint,
  `m_flag` string,
  `q_flag` string,
  `s_flag` string,
  `obs_time` bigint)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS INPUTFORMAT
'org.apache.hadoop.mapred.TextInputFormat'
OUTPUTFORMAT
'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION
's3://noaa-ghcn-pds/csv/by_year/'
TBLPROPERTIES (
  'areColumnsQuoted'='false',
  'classification'='csv',
  'columnsOrdered'='true',
  'compressionType'='none',
  'delimiter'=',',
  'skip.header.line.count'='1',
  'typeOfData'='file')
```