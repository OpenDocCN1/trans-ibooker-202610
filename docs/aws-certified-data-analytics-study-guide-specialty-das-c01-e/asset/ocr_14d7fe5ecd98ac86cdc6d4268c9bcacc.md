# Data Transformation with Amazon Kinesis Firehose

While capturing data is critical, you will often need to perform data transformations for data streaming at high velocity. You can enable data transformation with Amazon Kinesis Firehose, which will buffer incoming data to 3 MB by default. Kinesis Firehose will invoke the specified lambda function asynchronously, with each buffered batch using the AWS Lambda synchronous invocation mode. Any Lambda invocation has a maximum payload size limit of 6 MB for both the request and the response, and hence you have to ensure that the buffering size for the function is less than or equal to 6 MB.

You can also use pre-built Lambda blueprints with Kinesis Data Firehose. The following Lambda blueprints can be used for the most common transformations:

+   - General Firehose processing: Use this for any custom transformation logic.
+   - Apache Log to JSON
+   - Apache Log to CSV
+   - Syslog to JSON
+   - Syslog to CSV
+   - Kinesis Data Firehose processes record streams as a source: a basic lambda function that provides access to the Kinesis Data Stream records in the input and returns them with a processing status.
+   - Kinesis Data Firehose CloudWatch Logs processor: Parses and extracts each individual log event from your records sent by CloudWatch Logs subscription filters.

Each lambda function invocation from Kinesis Firehose can be a maximum of 5 minutes.

# Converting Input Record Formats in Amazon Kinesis

One of the most common analytical needs is to convert your row format into columnar to optimize your analytical queries. The most common columnar formats used by customers are either Parquet or ORC because they not only compress the data but also provide optimal performance for analytical queries. Amazon Kinesis Firehose allows you to convert JSON data into Parquet or ORC. Amazon Kinesis Firehose requires three elements to convert the record format of your data:

+   1. A deserializer to read the JSON of your input data: You can choose one of the two deserializers:
+      - Apache Hive JSON SerDe
+      - OpenX JSON SerDe
+   2. A schema to determine how to interpret your data: You can use AWS Glue to infer a schema from the data and store it in the Glue Data Catalog. Kinesis Firehose then references the schema and uses it to interpret your data.