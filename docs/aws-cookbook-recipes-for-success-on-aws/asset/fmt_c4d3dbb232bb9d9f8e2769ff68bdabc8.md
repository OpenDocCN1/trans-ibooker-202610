
5. From the left navigation menu, select Crawlers. Choose the crawler that you created in step 4 and click “Run crawler,” as shown in [Figure 7-14](#ch07.html#glue_crawler_configuration_summary).

#### Note

The Glue crawler will take a few moments to run. Once it is complete, you can view the table properties of the discovered schema and metadata.

![](assets/awsc_0714.png)

### Validation Checks

Verify the crawler configuration you created; you can use the AWS CLI or the Glue console. Note the `LastCrawl` status of `SUCCEEDED`:

```
aws glue get-crawler --name awscookbook703
```

You should see output similar to this:

```
{
  "Crawler": {
    "Name": "awscookbook703",
    "Role": "service-role/AWSGlueServiceRole-AWSCookbook703",
    "Targets": {
      "S3Targets": [
        {
          "Path": "s3://awscookbook704-<RANDOM_STRING>/data",
          "Exclusions": []
        }
      ],
      "JdbcTargets": [],
      "MongoDBTargets": [],
      "DynamoDBTargets": [],
      "CatalogTargets": []
    },
    "DatabaseName": "awscookbook703",
    "Classifiers": [],
    "RecrawlPolicy": {
      "RecrawlBehavior": "CRAWL_EVERYTHING"
    },
    "SchemaChangePolicy": {
      "UpdateBehavior": "UPDATE_IN_DATABASE",
      "DeleteBehavior": "DEPRECATE_IN_DATABASE"
    },
    "LineageConfiguration": {
      "CrawlerLineageSettings": "DISABLE"
    },
    "State": "READY",
    "CrawlElapsedTime": 0,
    "CreationTime": "2021-10-12T12:45:18-04:00",
    "LastUpdated": "2021-10-12T12:45:18-04:00",
    "LastCrawl": {
      "Status": "SUCCEEDED",
      "LogGroup": "/aws-glue/crawlers",
      "LogStream": "awscookbook703",
      "MessagePrefix": "16e867b7-e972-4ceb-b318-8e78370949d8",
      "StartTime": "2021-10-12T12:54:19-04:00"
    },
    "Version": 1
  }
}
```

#### Tip

AWS Glue crawlers log information about their runs to Amazon CloudWatch logs. If you need to debug your crawlers’ activity, you can inspect the logs in the `/aws-glue/crawlers` log group.

In the Glue console, select the table that was created and click “View properties.” You can also run an AWS CLI command to output the JSON:

```
aws glue get-table --database-name awscookbook703 --name data
```

The JSON properties should look similar to this:

```
{
  "StorageDescriptor": {
    "cols": {
      "FieldSchema": [
        {
          "name": "title",
          "type": "string",
          "comment": ""
        },
        {
          "name": "other titles",
          "type": "string",
          "comment": ""
        },
        {
          "name": "bl record id",
          "type": "bigint",
          "comment": ""
        }...<SNIP>...
      ]
    },
    "location": "s3://awscookbook703-<RANDOM_STRING>/data/",
    "inputFormat": "org.apache.hadoop.mapred.TextInputFormat",
    "outputFormat": "org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat",
    "compressed": "false",
    "numBuckets": "-1",
    "SerDeInfo": {
      "name": "",
      "serializationLib": "org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe",
      "parameters": {
        "field.delim": ","
      }
    },
    "bucketCols": [],
    "sortCols": [],
    "parameters": {
      "skip.header.line.count": "1",
      "sizeKey": "43017100",
      "objectCount": "1",
      "UPDATED_BY_CRAWLER": "awscookbook703",
      "CrawlerSchemaSerializerVersion": "1.0",
      "recordCount": "79367",
      "averageRecordSize": "542",
      "CrawlerSchemaDeserializerVersion": "1.0",
      "compressionType": "none",
      "classification": "csv",
      "columnsOrdered": "true",
      "areColumnsQuoted": "false",
      "delimiter": ",",
      "typeOfData": "file"
    },
    "SkewedInfo": {},
    "storedAsSubDirectories": "false"
  },
  "parameters": {
    "skip.header.line.count": "1",
    "sizeKey": "43017100",
    "objectCount": "1",
    "UPDATED_BY_CRAWLER": "awscookbook703",
    "CrawlerSchemaSerializerVersion": "1.0",
    "recordCount": "79367",
    "averageRecordSize": "542",
    "CrawlerSchemaDeserializerVersion": "1.0",
    "compressionType": "none",
    "classification": "csv",
    "columnsOrdered": "true",
    "areColumnsQuoted": "false",
    "delimiter": ",",
    "typeOfData": "file"
  }
}
```

### Cleanup

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/BigData).

## Discussion

When you start to ingest and store large amounts of data from various sources to object storage like S3, you may want to temporarily query the data in place without loading into an intermediate database. Since you may not always know the schema or metadata of the data, you need to know some basics about it, such as where the data resides, what the files and partitioning look like, whether it’s structured versus unstructured data, the size of data, and most importantly, the schema of the data. One specific feature of the AWS Glue service, Glue crawlers, allow you to discover metadata about the variety of data you have in storage. [Crawlers](https://oreil.ly/49nom) connect to your data source (in this case, S3 bucket), scan the objects in the source, and populate a Glue Data Catalog database with tables associated with your data’s schema.

#### Note

In addition to S3 bucket sources, you can use crawlers to scan Java Database Connectivity (JDBC) data stores and DynamoDB tables. In the case of a JDBC table, you will need to define a [connection](https://oreil.ly/Gasxy) to allow Glue to use a network connection to your JDBC source.

### Challenge

Configure your crawler to run on an interval so that your tables and metadata are automatically updated.

# 7.4 Querying Files on S3 Using Amazon Athena

## Problem

You need to run a SQL query on CSV files stored on object storage without indexing them.

## Solution

Configure an Amazon Athena results S3 bucket location, create a Data Catalog database and table in the Athena Editor, and run a SQL query on the data in the S3 bucket, as shown in [Figure 7-15](#ch07.html#query_files_on_sthree-id0000044).

![](assets/awsc_0715.png)

### Prerequisite

*   S3 bucket with a CSV file containing data

### Preparation

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/BigData).

### Steps

1.  Log into the AWS Console and go to the Athena console; you should see something similar to [Figure 7-16](#ch07.html#athena_console).

    ![](assets/awsc_0716.png)

2.  In the Query Editor, click the Settings tab and configure a Query result location using the S3 bucket that you created and a prefix: `s3://bucket/folder/object/`. Click Manage, select the bucket, and click Choose. As an option, you can encrypt the results. See [Figure 7-17](#ch07.html#athena_results_destination_configuratio) for an example.

    ![](assets/awsc_0717.png)

3.  Back in the Editor tab, run the following SQL statement to create a Data Catalog database:

    ```
    CREATE DATABASE `awscookbook704db`
    ```

    You should see output similar to [Figure 7-18](#ch07.html#database_creation_sql_statement).

    ![](assets/awsc_0718.png)

4.  Run a new statement in the Query Editor to create a table within the database that references the S3 bucket location of the data and the schema of the data. Be sure to replace `BUCKET_NAME` with the name of the bucket that you created:
