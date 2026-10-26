C. Amazon MSK
D. Shards

# 24. Which of the following statements are true about KPL (Kinesis Producer Library)? (Choose three.)

+   A. Writes to one or more Kinesis Data Streams with an automatic and configurable retry mechanism.
+   B. Aggregates user records to increase payload size.
+   C. Submits CloudWatch metrics on your behalf to provide visibility into producer performance.
+   D. Forces the caller application to block and wait for a confirmation.
+   E. KPL does not incur any processing delay and hence is useful for all applications writing data to a Kinesis stream.
+   F. RecordMaxBufferedTime within the library is set to 1 millisecond and not changeable.

# 25. Which of the following is true about Kinesis Client Library? (Choose three.)

+   A. KCL is a Java library and does not support other languages.
+   B. KCL connects to the data stream and enumerates the shards within the data stream.
+   C. KCL pulls data records from the data stream.
+   D. KCL does not provide a checkpointing mechanism.
+   E. KCL instantiates a record processor for each stream.
+   F. KCL pushes the records to the corresponding record processor.

# 26. Which of the following metrics are sent by the Amazon Kinesis Data Streams agent to Amazon CloudWatch? (Choose three.)

+   A. MBs Sent
+   B. RecordSendAttempts
+   C. RecordSendErrors
+   D. RecordSendFailures
+   E. ServiceErrors
+   F. ServiceFailures

# 27. You are working as a data engineer for a gaming startup, and the operations team notified you that they are receiving a ReadProvisionedThroughputExceeded error. They are asking you to help out and identify the reason for the issue and help in the resolution. Which of the following statements will help? (Choose two.)

+   A. The GetRecords calls are being throttled by KinesisDataStreams over a duration of time.
+   B. The GetShardIterator is unable to get a new shard over a duration of time.
+   C. Reshard your stream to increase the number of shards.
+   D. Redesign your stream to increase the time between checks for the provision throughput to avoid the errors.