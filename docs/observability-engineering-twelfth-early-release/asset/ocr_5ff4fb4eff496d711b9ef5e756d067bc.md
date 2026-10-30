+   - Filter low-value logs from being sent to the backend

For trace data in particular at Slack, one way we ensure data consistency is by using simple data-filtering operations like filtering low-value spans from making it to the backend, increasing the overall value of the data set. Other examples of ensuring quality include techniques like tail sampling, in which only a small subset of the reported traces are selected for storage in the backend system based on desirable attributes, such as higher reported latency.

# Managing a Telemetry Pipeline: Anatomy

In this section, we cover the basic components and architecture of a functional telemetry pipeline. Simply stated, a *telemetry pipeline* is a chain of receiver, buffer, processor and exporter components, all in a series.

The following are details of these key components of a telemetry pipeline:

+   - A *receiver* collects data from a source.
+     A receiver can collect the data directly from applications like a Prometheus scraper. Alternatively, a receiver can also ingest data from a buffer.
+     A receiver can expose an HTTP API to which the applications can push their data.
+     A receiver can also write to a buffer to ensure data integrity during disruptions.

+   - A *buffer* is a store for the data, often for a short period of time.
+     A buffer holds the data temporarily until it can be consumed by a backend or downstream application.
+     Often a buffer is a pub-sub system like Kafka or Amazon Kinesis.
+     An application can also push the data directly to a buffer. In such cases, a buffer also acts as a source of the data for a receiver.