
*   Real-time financial market data
*   IoT and sensor data
*   End-user clickstream activity from web and mobile applications

You can use the Kinesis Producer Library (KPL) and Kinesis Client Library (KCL) within your application for your specific needs. When consuming data from Kinesis streams, you can configure your application to read records from the stream and respond, invoke Lambda functions directly from the stream, or even use a [Kinesis Data Analytics](https://oreil.ly/lyR4E) application (powered by Apache Flink) directly within the Kinesis service.

**Note**

The Kinesis service will scale automatically to meet your needs, but you need to be aware of the [quotas and limits](https://oreil.ly/f7kq3) to ensure that you do not exceed capacity with your shard configuration.

### Challenge

Automatically trigger a Lambda function to process incoming Kinesis data.

# 7.2 Streaming Data to Amazon S3 Using Amazon Kinesis Data Firehose

## Problem

You need to deliver incoming streaming data to object storage.

## Solution

Create an S3 bucket, create a Kinesis stream, and configure Kinesis Data Firehose to deliver the stream data to the S3 bucket. The flow is shown in [Figure 7-2](#ch07.html#streaming_data_to_amazon-id0000048).

![](assets/awsc_0702.png)

### Prerequisites

*   Kinesis stream
*   S3 Bucket with a CSV file

### Preparation

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/BigData).

### Steps

1.  Open the [Kinesis Data Firehose console](https://oreil.ly/GG8yA) and click the “Create delivery stream” button; choose Amazon Kinesis Data Streams for the source and Amazon S3 for the destination, as shown in [Figure 7-3](#ch07.html#kinesis_data_firehose_choose_source_and).

    ![](assets/awsc_0703.png)

2.  For Source settings, choose the Kinesis stream that you created in the preparation steps, as shown in [Figure 7-4](#ch07.html#choosing_a_kinesis_data_stream).

    ![](assets/awsc_0704.png)

3.  Keep the defaults (Disabled) for “Transform and convert records” options. For Destination settings, browse for and choose the S3 bucket that you created in the preparation steps as shown in [Figure 7-5](#ch07.html#kinesis_data_firehose_destination_confi), and keep the defaults for the other options (disabled partitioning and no prefixes).

    ![](assets/awsc_0705.png)

4.  Under the Advanced settings section, confirm that the “Create or update IAM role” is selected. This will create an IAM role that Kinesis can use to access the stream and S3 bucket, as shown in [Figure 7-6](#ch07.html#creating_an_iam_role_for_the_kinesis_da).

    ![](assets/awsc_0706.png)

#### Validation checks

You can test delivery to the stream from within the Kinesis console. Click the Delivery streams link in the left navigation menu, choose the stream you created, expand the “Test with demo data” section, and click the “Start sending demo data” button. This will initiate sending sample data to your stream so you can verify that it is making it to your S3 bucket. A sample is shown in [Figure 7-7](#ch07.html#sending_test_data_through_a_kinesis_dat).

![](assets/awsc_0707.png)

After a few minutes, you will see a folder structure and a file appear in your S3 bucket, similar to [Figure 7-8](#ch07.html#sthree_destination_with_kinesis_stream).

![](assets/awsc_0708.png)

If you download and inspect the file, you will see output similar to the following:

```
{"CHANGE":3.95,"PRICE":79.75,"TICKER_SYMBOL":"SLW","SECTOR":"ENERGY"}
{"CHANGE":7.27,"PRICE":96.37,"TICKER_SYMBOL":"ALY","SECTOR":"ENERGY"}
{"CHANGE":-5,"PRICE":81.74,"TICKER_SYMBOL":"QXZ","SECTOR":"HEALTHCARE"}
{"CHANGE":-0.6,"PRICE":98.4,"TICKER_SYMBOL":"NFLX","SECTOR":"TECHNOLOGY"}
{"CHANGE":-0.46,"PRICE":18.92,"TICKER_SYMBOL":"PLM","SECTOR":"FINANCIAL"}
{"CHANGE":4.09,"PRICE":100.46,"TICKER_SYMBOL":"ALY","SECTOR":"ENERGY"}
{"CHANGE":2.06,"PRICE":32.34,"TICKER_SYMBOL":"PPL","SECTOR":"HEALTHCARE"}
{"CHANGE":-2.99,"PRICE":38.98,"TICKER_SYMBOL":"KFU","SECTOR":"ENERGY"}
```

### Cleanup

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/BigData).

## Discussion

As you begin to ingest data from various sources, your application may be consuming or reacting to streaming data in real time. In some cases, you may want to store the data from the stream to query or process it later. You can use Kinesis Data Firehose to deliver data to object storage (S3), Amazon Redshift, OpenSearch, and many third-party endpoints. You can also connect multiple delivery streams to a single producer stream to deliver data if you have to support multiple delivery locations from your streams.

**Note**

Kinesis Data Firehose scales automatically to handle the volume of data you need to deliver, meaning that you do not have to configure or provision additional resources if your data stream starts to receive large volumes of data. For more information on Kinesis Data Firehose features and capabilities, see the [AWS documentation](https://oreil.ly/9h2Bc) for Kinesis Data Firehose.

If you need to transform data before it ends up in the destination via Firehose, you can configure transformations. A transformation will automatically invoke a Lambda function as your streaming data queues up (see this Firehose [article](https://oreil.ly/PW0LN) for buffer size information). This is useful when you have to adjust the schema of a record before delivery, sanitize data for long-term storage on the fly (e.g., remove personally identifiable information), or join the data with other sources before delivery. The transformation Lambda function you invoke must follow the convention specified by the Kinesis Data Firehose API. To see some examples of Lambda functions, go to the [AWS Serverless Application Repository](https://oreil.ly/q7noh) and search for “firehose.”

### Challenge

Configure a Firehose delivery with transformations to remove a field from the streaming data before delivery.

# 7.3 Automatically Discovering Metadata with AWS Glue Crawlers

## Problem

You have CSV data files on object storage, and you would like to discover the schema and metadata about the files to use in further analysis and query operations.

## Solution

Create an AWS Glue database, follow the crawler configuration wizard to configure a crawler to scan your S3 bucket data, run the crawler, and inspect the resulting table, as shown in [Figure 7-9](#ch07.html#automatically_discover_me-id0000027).

![](assets/awsc_0709.png)

### Prerequisite

*   An S3 bucket

### Preparation

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/BigData).

### Steps

1.  Log in to the AWS Console and navigate to the AWS Glue console, choose Databases from the left navigation menu and select “Add database,” as shown in [Figure 7-10](#ch07.html#creating_a_database_in_the_glue_data_ca).

    ![](assets/awsc_0710.png)

2.  Give your database a name (e.g., `awscookbook703`) and click Create. A sample dialog box is shown in [Figure 7-11](#ch07.html#database_name_dialog_box).

    ![](assets/awsc_0711.png)

3.  Select Tables from the left navigation menu and choose “Add tables” → “Add tables using a crawler,” as shown in [Figure 7-12](#ch07.html#adding_a_table_to_the_glue_data_catalog).

    ![](assets/awsc_0712.png)

4.  Follow the “Add crawler” wizard. For the crawler source type, choose “Data stores,” crawl all folders, S3 as “Data store” (do not define a Connection), choose your S3 bucket and “data” folder in “Include path,” and do not choose a sample size. Choose to create an IAM role, and suffix it with `AWSCookbook703`. For a frequency, choose “Run on demand,” and select the database you created in step 2. Confirm the configuration on the “Review all steps” page and click Finish. An example review page is shown in [Figure 7-13](#ch07.html#review_settings_for_glue_crawler).

    ![](assets/awsc_0713.png)
