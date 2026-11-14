
You made use of the `awslogs` driver and an IAM role, which allows the running task to write to a `CloudWatch` log group. This is a common pattern when working with containers on AWS, as you most likely need log output for troubleshooting and debugging your application. This configuration is handled by tools like `Copilot` since it is a common pattern, but when working with `Amazon ECS` directly, like defining and running a task, the configuration is critical for developers to know about.

The `PID 1` [process output](https://oreil.ly/XY2UF) to `/dev/stdout` and `/dev/stderr` is captured by the `awslogs` driver. In other words, the first process in the container is the only process logging to these streams. Be sure your application that you would like to see logs from is running with `PID 1` inside of your container.

In order for most AWS services to communicate with one another, you must assign a role to them that allows the required level of permissions for the communication. This holds true when configuring logging to `CloudWatch` from a container ECS task; the container must have a role associated with it that allows the `CloudWatchLogs` operations via the [`awslogs logConfiguration` driver](https://oreil.ly/OrzrZ):

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogStreams"
      ],
      "Resource": [
        "arn:aws:logs:*:*:*"
      ]
    }
  ]
}
```

`CloudWatch` logs allow for a central logging solution for many AWS services. When running multiple containers, it is important to be able to quickly locate logs for debugging purposes.

### Challenge

You can [`tail`](https://oreil.ly/kFrGf) the logs of a log stream to give you a more real-time view of the logs that your application is generating. This can help in your development and troubleshooting activities. Try using the `aws logs tail` command with your log stream while generating some output for you to observe.

# Chapter 7. Big Data

## 7.0 Introduction

Data is sometimes referred to as “[the new gold](https://oreil.ly/6EDB0).” Many companies are leveraging data in new and exciting ways every day as available data science tools continue to improve. You can now mine troves of historical data quickly for insights and patterns by using modern analytics tools. You might not yet know the queries and analysis you need to run against the data, but tomorrow you might be faced with a challenge that could be supported by historical data analysis using new and emerging techniques. With the advent of cheaper data storage, many organizations and individuals opt to keep data rather than discard it so that they can run historical analysis to gain business insights, discover trends, train AI/ML models, and be ready to implement future technologies that can use the data.

In addition to the amount of data you might collect over time, you are also collecting a wider variety of data types and structures at an increasingly faster velocity. Imagine that you might deploy `IoT` devices to collect sensor data, and as you continue to deploy these over time, you need a way to capture and store the data in a scalable way. This can be structured, semistructured, and unstructured data with schemas that might be difficult to predict as new data sources are ingested. You need tools to be able to transform and analyze your diverse data.

An informative and succinct AWS re:Invent 2020 presentation by Francis Jayakumar, “[An Introduction to Data Lakes and Analytics on AWS](https://oreil.ly/WcmSj),” provides a high-level introduction to what is available on AWS for big data and analytics. We could have included so much in this chapter—enough to fill another book—but we will focus on foundational recipes for sending data to `S3`, discovering data on `S3`, and transforming data to give you examples of working with data on AWS.

### Workstation Configuration

You will need a few things installed to be ready for the recipes in this chapter.

#### General setup

Follow the [“General workstation setup steps for CLI recipes”](#preface01.html#general_workstation_setup_steps_for_cli) to validate your configuration and set up the required environment variables. Then, clone the chapter code repository:

```
git clone https://github.com/awscookbook/BigData
```

## 7.1 Using a Kinesis Stream for Ingestion of Streaming Data

### Problem

You need a way to ingest streaming data for your applications.

### Solution

Create a `Kinesis` stream and verify that it is working by using the `AWS CLI` to put a record on the stream, as shown in [Figure 7-1](#ch07.html#using_a_kinesis_stream_for-id0000050).

![](assets/awsc_0701.png)

#### Steps

1.  Create a Kinesis stream:

    ```
    aws kinesis create-stream --stream-name AWSCookbook701 --shard-count 1
    ```

    ##### Note

    [Shards](https://oreil.ly/XKI0q) are an important concept to understand with Kinesis streams as you scale your stream to handle larger velocities of incoming streaming data. Each shard can support up to five transactions per second for reads, up to a maximum total data read rate of 2 MB per second. For writes, each shard can support up to 1,000 records per second, up to a maximum total data write rate of 1 MB per second (including partition keys). You can re-shard your stream at any time if you need to handle more data.

2.  Confirm that your stream is in `ACTIVE` state:

    ```
    aws kinesis describe-stream-summary --stream-name AWSCookbook701
    ```

    You should see output similar to the following:

    ```
    {
      "StreamDescriptionSummary": {
        "StreamName": "AWSCookbook701",
        "StreamARN": "arn:aws:kinesis:us-east-1:111111111:stream/AWSCookbook701",
        "StreamStatus": "ACTIVE",
        "RetentionPeriodHours": 24,
        "StreamCreationTimestamp": "2021-10-12T17:12:06-04:00",
        "EnhancedMonitoring": [
          {
            "ShardLevelMetrics": []
          }
        ],
        "EncryptionType": "NONE",
        "OpenShardCount": 1,
        "ConsumerCount": 0
      }
    }
    ```

#### Validation checks

Put a record on the Kinesis stream:

```
aws kinesis put-record --stream-name AWSCookbook701 \
     --partition-key 111 \
     --cli-binary-format raw-in-base64-out \
     --data={\"Data\":\"1\"}
```

You should see output similar to the following:

```
{
  "ShardId": "shardId-000000000000",
  "SequenceNumber": "49622914081337086513355510347442886426455090590105206786"
}
```

Get the record from the Kinesis stream. [Get the shard iterator](https://oreil.ly/dFx8m) and run the `get-records` command:

```
SHARD_ITERATOR=$(aws kinesis get-shard-iterator \
     --shard-id shardId-000000000000 \
     --shard-iterator-type TRIM_HORIZON \
     --stream-name AWSCookbook701 \
     --query 'ShardIterator' \
     --output text)
aws kinesis get-records --shard-iterator $SHARD_ITERATOR \
     --query Records[0].Data --output text | base64 --decode
```

You should see output similar to the following:

```
{"Data":"1"}
```

##### Note

The data is base64 encoded that you published to the stream. You queried the command output for the data element within the JSON object and piped the output to `base64 --decode` to validate that the record is what you published.

### Cleanup

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/BigData).

### Discussion

Streaming data can come from a variety of sources. The sources putting records on streams are known as [producers](https://oreil.ly/ze6Wd). Entities getting records from streams are known as [consumers](https://oreil.ly/dzRG2). In the case of streaming data, you are dealing with real-time information, and you may need to act on it immediately or store it for usage later (see [Recipe 7.2](#ch07.html#streaming_data_to_amazon-id0000022)). Some common producer examples to think about are as follows:
