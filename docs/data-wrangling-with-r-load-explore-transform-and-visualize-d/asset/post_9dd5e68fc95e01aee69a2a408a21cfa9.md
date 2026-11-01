
the option of using AWS Glue ETL for data ingestion from various data sources, such as data lakes (Amazon S3), databases, streaming, and SaaS data stores. Additionally, we will learn about how to use job bookmarks to perform incremental data loads from Data Lake (S3) and JDBC.

Glue enables users to create ETL jobs using three different types of ETL frameworks – Spark ETL, Spark Streaming, and Python Shell. In the introduction section of Glue DataBrew, we learned how AWS Glue has evolved and that now, AWS Glue Studio is available to build ETL pipelines.

The AWS Glue user interface allows you to build your ETL pipeline with an interesting feature that converts your UI job into a script, which helps you scale when building similar pipelines or when you need some help to get started and then add customization.

In addition to supporting multiple Spark frameworks to process data, Glue provides advanced features during job creation, such as auto-scaling, job bookmarking, enabling continuous logging, the Spark UI, Flex instances, and passing Spark and job parameters.

In addition to the Spark framework, Glue ETL provides Glue-native advanced frameworks/APIs and formats for complex ETL, such as GlueContext, DynamicFrame, job bookmarking, Glue Parquet, Glue Interactive session, workflows, triggers, and more.

Let’s explore these features one by one.

## AWS GlueContext

The GlueContext class wraps Apache’s SparkContext and can be initialized using the following code snippet:

```
sc = SparkContext()
glueContext = GlueContext(sc)
```

GlueContext allows you to communicate with the Spark framework and is the entry point for many Glue ETL features, such as DynamicFrame, job bookmarking, job parameters, and more.

## DynamicFrame

Glue ETL’s DynamicFrame is a vital feature that enables Spark developers to create more efficient Spark pipelines, and it is native to Glue. A Glue DynamicFrame is a distributed collection of self-describing DynamicRecord objects, but it does not require them to adhere to a fixed schema. Due to the self-describing nature of records in DynamicFrame, it does not require a schema to be created and can be used to read/transform data with inconsistent schemas.

You can create a Glue DynamicFrame using different APIs, depending on your use case. For example, the following code snippet creates a DynamicFrame object called `datasource0` for the `my_table` table in the `my_database` database:

```
datasource0 = glueContext.create_dynamic_frame.from_catalog(database_name='my_database',
table_name='my_table', transformation_ctx='datasource0')
```

In this statement, we used the `GlueContext` object, which uses the Glue SDK, to connect to Glue Data Catalog and fetch the data source classifications and properties to create the object. Users can also pass additional options using the additional `_options` parameter, such as the `push_down_predicate` parameter.

Besides using the `from_catalog` method to create a DynamicFrame, you can create DynamicFrames using the following methods:

- **from_options**: This method allows users to create DynamicFrames by manually specifying the connection type, options, and format. This method provides users with the flexibility to customize options for a data store.
- **from_rdd**: This method allows users to create DynamicFrames using Spark RDDs.

To leverage the features available in DataFrame, you can convert a DynamicFrame into a DataFrame using the following syntax:

```
df = datasource0.toDF()
```

Similarly, you can convert a Spark DataFrame into a Glue DynamicFrame using the following syntax:

```
from awsglue.dynamicframe import DynamicFrame
dyf = DynamicFrame.fromDF(dataframe=df,
glue_ctx=glueContext, name="dyf")
```

Both DynamicFrame and DataFrame are high-level APIs that interact with Spark RDDs. However, the structure of a DynamicFrame is different from a DataFrame:

![](img/ec1933a597ce63afa3bcf7496364f412_213_0.png)

Figure 6.22 – AWS Glue ETL high-level architecture

It is important to note that converting a Glue DynamicFrame into a Spark DataFrame requires a full Map stage in Spark, which can be resource-intensive and add extra time to your job execution time. This is because converting a DynamicFrame into a DataFrame blocks Spark, using all the optimization done against DynamicFrame.

## AWS Glue Job bookmarks

AWS Glue offers the unique capability of performing bookmarking, which distinguishes it from other Spark-based ETL solutions such as running Spark on Amazon **Elastic MapReduce (EMR)**. Bookmarking enables incremental data loading by tracking the data that has already been processed and written. This is particularly useful when dealing with large datasets or extracting data from databases.

Job bookmarks are available for JDBC data sources, Amazon S3, and relational transforms. For Amazon S3, job bookmarks are available for the JSON, CSV, Apache Avro, XML, Parquet, and ORC file formats. When working with JDBC sources, users need to specify a column as the bookmark key; otherwise, AWS Glue will use the primary key as the bookmark key, provided that it is sequentially increasing or decreasing without gaps. If user-defined bookmark keys are used, they should be monotonically increasing or decreasing; gaps are permitted in this case.

For Amazon S3, AWS Glue uses the last modified time of the objects to determine which objects need to be reprocessed. There are various processing options available, such as resetting job bookmarks and rewinding. Resetting job bookmarks allows users to reprocess all the data using the same job, while rewinding enables them to go back to any previous job run. It is important to note that converting a Glue DynamicFrame into a Spark DataFrame requires a full Map stage in Spark,

which can be resource-intensive and add extra time to job execution due to blocking Spark optimizations against DynamicFrames.

## AWS Glue Triggers

AWS Glue Triggers enable you to schedule your Glue workflow, which includes Glue jobs and crawlers. You can create triggers that fire on-demand, scheduled, or based on events. Triggers can exist in several states, such as CREATED, ACTIVATED, and DEACTIVATED.

Three types of triggers are available:

- **Scheduled triggers:** These triggers are time-based and can be configured using cron expressions to define the schedule.
- **Conditional triggers:** These triggers execute based on the previous job or crawler states. For example, you can configure a trigger to execute Job 1 only when a crawler has completed.
- **On-demand triggers:** These triggers are activated manually, as the name suggests, and execute when you run them.

## AWS Glue interactive sessions

AWS Glue provides an interactive environment for building, testing, and running your ETL jobs on AWS. This environment includes an open source Jupyter kernel that integrates with popular IDEs such as PyCharm, IntelliJ, and VS Code, allowing you to write and test code locally. To set up an interactive session, you can refer to the official Glue documentation at https://docs.aws.amazon.com/glue/latest/dg/interactive-sessions.html.

Now that we have a fair understanding of all the Glue components, let’s learn about some ingestion patterns through examples:

- Ingesting data from Amazon S3 (Object Store)
- Ingesting data from SaaS applications
- Ingesting data from relational databases

## AWS Glue Studio

Glue Studio is a powerful visual interface provided by AWS Glue for building and managing ETL workflows. It offers several features and benefits that enhance the data integration and transformation process. However, there are also some limitations and considerations to keep in mind. Let’s provide an overview.

The features and benefits of Glue Studio are as follows:

- **Visual ETL development**: Glue Studio provides a user interface for visually designing ETL workflows. It simplifies the process of building data pipelines by allowing users to create, connect, and configure pre-built components called “transforms” without writing code.
- **Pre-built transform library**: Glue Studio offers a rich library of pre-built transforms, which are data processing operations such as filtering, aggregating, joining, and more. These transforms can be easily incorporated into the workflow, saving time and effort in developing complex data transformations.
- **Auto-generated Spark code**: Behind the scenes, Glue Studio automatically generates Apache Spark code based on the visual workflow. This enables users to leverage the power of Spark for distributed data processing while abstracting away the complexities of Spark programming.
- **Integration with Glue Data Catalog**: Glue Studio seamlessly integrates with Glue Data Catalog, allowing easy access to metadata, table definitions, and schemas. It simplifies the process of working with data stored in various AWS data sources such as Amazon S3, RDS, Redshift, and more.
- **Built-in data catalog and data lake**: Glue Studio leverages Glue Data Catalog as a central repository for managing and organizing metadata. It enables users to discover, catalog, and explore data assets within their data lake, promoting data governance and self-service data discovery.
- **Easy deployment and scheduling**: Glue Studio provides simple deployment options, allowing users to deploy their workflows as AWS Glue ETL jobs with just a few clicks. It also offers scheduling capabilities to automate the execution of workflows repeatedly.

One of the limitations and considerations of AWS Glue Studio is **limited customization**. While Glue Studio provides a visual interface for ETL development, it may have limitations when it comes to complex transformations or custom logic. In such cases, you may need to switch to traditional coding approaches using Glue ETL scripts or other AWS services.

## Ingesting data from object stores

This example involves copying data from the COVID-19 data lake on Open Data on AWS (https://registry.opendata.aws/aws-covid19-lake/), which is stored in a public S3 bucket named `s3://covid19-lake/`. To achieve this, we will use the Amazon S3 Boto3 library, a Python package that’s used to connect to data in different file stores such as SFTP, FTP, and others. In this case, a Glue Python shell ETL job will be utilized to read CSV data from Amazon S3, convert it into JSON, and then copy it to a target S3 bucket. The COVID-19 data lake on Amazon S3 contains various folders, and in this example, the `s3://covid19-lake/rearc-covid-19-testing-data/csv/us_daily/` folder will be used:

```
(base) ~ % aws s3 ls s3://covid19-lake/rearc-covid-19-testing-data/csv/us_daily/
2021-03-15 15:34:30       82926 us_daily.csv
```

Here is the code snippet:

```
import boto3
import io
import pandas as pd
```
