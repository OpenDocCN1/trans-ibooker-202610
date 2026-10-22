detecting anomalies. Within the Framework, Dataflow will read data from Pub/Sub, call the Speech-to-Text and Natural Language APIs, enrich the data, and finally write it to BigQuery. Google donated the Google Cloud Dataflow SDK to the Apache Software Foundation in 2016, and other organizations have contributed runners and IOs to integrate Beam runners with existing Databases which has allowed the project to grow in features and community support.

Apache Beam is a programming model that defines and executes the defined pipeline. The pipelines can be batch and streaming which are exposed to different runners as:

+   - Google Cloud Dataflow
+   - Apache Spark
+   - Apache Flink
+   - Apache Apex
+   - DirectRunner (a local runner for testing)

The idea is that the same pipelines can be executed on different runners allowing for probability and choice of what runner to use. For the Framework, you will use the Google Cloud Dataflow Runner. Choosing Cloud Dataflow as the runner allows you to focus on your pipeline and allows Cloud Dataflow to manage the resources on-demand and autoscale to achieve minimal latency and high resource utilization. When running the pipeline in Google Cloud the runner uploads the executable code and dependencies to Cloud Storage then creates a Dataflow job that executes your pipeline. You can set the runner programmatically, or specify it using the command-line. The following example shows how to build a pipeline by programmatically setting the runner to Dataflow.

```
options = PipelineOptions(
    flags=argv,
    runner='DataflowRunner',
    project='my-project-id',
    job_name='unique-job-name',
```