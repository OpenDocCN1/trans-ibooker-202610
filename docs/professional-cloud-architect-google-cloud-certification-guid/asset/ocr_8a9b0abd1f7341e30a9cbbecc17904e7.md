# Exporting logs

Log entries that are received by logging can be exported (copied) to Cloud Storage buckets, BigQuery data sets, and Cloud Pub/Sub topics. You export logs by configuring log sinks, which then continue to export log entries as they arrive in logging. A sink includes a destination and a filter that selects the log entries to be exported. Remember that only the logs that were created after the sink configuration will be exported:

+   1. To set up an export, go to Logs Router in the Cloud Logging window and click on CREATE SINK:

![](bbox=[0.13, 0.29, 0.86, 0.58])

Figure 17.23 – Logs Router

+   2. In Sink details, fill in the name of the sink and description:

![](bbox=[0.25, 0.65, 0.74, 0.88])

Figure 17.24 – Sink details