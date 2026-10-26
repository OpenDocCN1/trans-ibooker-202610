
## Data Transformation with Amazon Kinesis Firehose

While capturing data is critical, you will often need to perform data transformations for data streaming at high velocity. You can enable data transformation with Amazon Kinesis Firehose, which will buffer incoming data up to 3 MB by default. Kinesis Firehose will invoke the specified Lambda function synchronously, with each buffered batch using the AWS Lambda synchronous invocation mode. Any Lambda invocation has a maximum payload size limit of 6 MB for both the request and the response, and hence you have to ensure that the buffering size for the function is less than or equal to 6 MB.

You can also use pre-built Lambda blueprints with Kinesis Data Firehose. The following Lambda blueprints can be used for the most common transformations:

- General Firehose processing: Use this for any custom transformation logic.
- Apache Log to JSON
- Apache Log to CSV
- Syslog to JSON
- Syslog to CSV
- Kinesis Data Firehose processes record streams as a source: a basic Lambda function that provides access to the Kinesis Data Stream records in the input and returns them with a processing status.
- Kinesis Data Firehose CloudWatch Logs processor: Parses and extracts each individual log event from your records sent by CloudWatch Logs subscription filters.

Each Lambda function invocation from Kinesis Firehose can be a maximum of 5 minutes.

## Converting Input Record Formats in Amazon Kinesis

One of the most common analytical needs is to convert your row format into columnar to optimize your analytical queries. The most common columnar formats used by customers are either Parquet or ORC because they not only compress the data but also provide optimal performance for analytical queries. Amazon Kinesis Firehose allows you to convert JSON data into Parquet or ORC. Amazon Kinesis Firehose requires three elements to convert the record format of your data:

1. A deserializer to read the JSON of your input data: You can choose one of the two deserializers:
   - Apache Hive JSON SerDe
   - OpenX JSON SerDe
2. A schema to determine how to interpret your data: You can use AWS Glue to infer a schema from the data and store it in the Glue Data Catalog. Kinesis Firehose then references the schema and uses it to interpret your data.

You can also use an Apache Flink interface for your Kinesis Data Analytics application. Essentially you would create your Flink application in your favorite integrated development environment (IDE) and then upload the application code to Kinesis Data Analytics. This would allow you to run your application in a fully managed elastic service. You can then use the AWS ecosystem to manage your application. For example, you can configure various metrics with Amazon CloudWatch—such as Applications, Tasks, Operator, and Parallelism—and also display the Job graph in the AWS Management Console.

Kinesis provides a Kinesis processing unit (KPU) as a resource to run the code. Each KPU is 1 vCPU, 4 GB of RAM, and 50 GB of storage at the time of this writing. The number of KPUs needed for your application is calculated using the Parallelism and ParallelismPerKPU properties:

- Parallelism – This is the property that is used to set the default Apache Flink application parallelism. All operators, sinks, and sources will use this level of parallelism unless they are explicitly overridden in the application. The default value is 1 and the maximum value is 64.

- ParallelismPerKPU – This property is used to set the number of parallel tasks that can be scheduled per KPU of your application. The default is 1 and the maximum is 8.

Please note that the limits are soft limits and can be increased by requesting a limit increase.

You can use the autoscaling capability of your Kinesis application to scale your resource up and down depending on the requirements and throughput needed (Figure 2.19).

### Figure 2.19 Kinesis Data Analytics - Flink Interface

![](img/fc6a7083d65b0716921d7bd4a5298460_100_0.png)

You can configure parallelism and parallelism per KPU in addition to enabling autoscaling on the AWS console while configuring your Kinesis Data Analytics application (Figure 2.20).

### Figure 2.20 Scaling your Flink applications with Kinesis

![](img/fc6a7083d65b0716921d7bd4a5298460_101_0.png)

## Amazon Kinesis Video Streams

With the advent of IoT and integrated devices that capture other measurements you can now capture video data, which needs to be stored, analyzed, and reacted to in a real-time fashion. Video is critical to many applications like smart home, smart city, video surveillance security monitoring, industrial automation, and computer vision. While the requirements to stream, process, and analyze video in real time has grown, building such a video ingestion system is quite complex to say the least. As it turns out, there are many different devices and different operating systems you need to integrate with, and writing code for various devices and development environments is very complex. Furthermore, as the number of devices grows, there is a need to scale your streaming and analysis applications while supporting the latency, cadence, and jitter on the streams. APIs need to be made available to retrieve the videos, process them, and replay them.

Video streams analysis was one of the most common requests from media customers, and Amazon responded by releasing Amazon Kinesis Video Streams at the re:Invent event in 2017, with the key objective to stream video and time-encoded data for analytics. Amazon Kinesis Video Streams is a fully managed service that is scalable and allows you to stream videos from millions of sources, including smart phones, security cameras, webcams, drones, cameras embedded in cars, and other sources. You can also send non-video time-serialized data such as audio data, thermal imagery, depth data, RADAR (Radio Detection and Ranging) data, and more. As live video streams from these sources into a Kinesis video stream, you can build applications that can access the data, frame-by-frame, in real time for low-latency processing. Amazon Kinesis Video Streams is source-agnostic; you can stream video from a computer’s webcam using the GStreamer library or from a camera on your network using RTSP.

Amazon Kinesis Video Streams allows you to not only capture this data but also easily build vision-enabled applications. It provides durable storage where you can configure the retention period, encrypt the data at rest, and time-stamp the data based on the producer time stamp and the ingestion time stamp (Figure 2.21). The data stored is searchable and allows you to integrate with various downstream services like Amazon AI services, Apache MXNet, TensorFlow, and other third-party providers. The key benefits of Kinesis Video Streams for a customer are as follows:

- Connect and stream from millions of devices.
- Store and index the data in a durable manner.
- Focus on the business case rather than the infrastructure management.
- Pay-as-you-go model for pricing.
- Stream your data in a secure manner.
- Build real-time and batch applications.

The key concepts are as follows:

- **Producer** – Producer is any application, device, or source that feeds the data into Kinesis Video Streams. A producer can be a video-generating device or even send non-video data like audio streams, images, or RADAR data.

- **Kinesis Video Producer Libraries** – These libraries are supposed to be installed on your device and allow you to connect to the Kinesis stream and stream video to the stream in real time or with some buffering.

- **Kinesis Video Streams** – The producers put the videos in a Kinesis video stream, which is basically a resource that enables you to transport live video data, optionally store it, and make the data available for consumption both in real time and on a batch or ad hoc basis. You can create a Kinesis video stream from the AWS Console or using the AWS SDK (see Figure 2.22).
