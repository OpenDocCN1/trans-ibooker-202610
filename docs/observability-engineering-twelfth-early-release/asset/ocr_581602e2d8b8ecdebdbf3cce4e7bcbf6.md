advanced approaches include pre-aggregating the data in the high-cardinality field(s), or dropping data with invalid data (like incorrect or malformed timestamps).

In the case of logs, you would want to filter PII data, filter security data like security tokens, or sanitize URLs since often the logging system is not meant to store sensitive data. For trace data, you might like to additionally sample high-value traces or drop low-value spans from making it to the backend system.

In addition to filtering data, the telemetry pipeline can also be used to enrich the telemetry data for better usability. Often this includes adding additional metadata that is available outside the process, like region information or Kubernetes container information; resolving IPs to their hostnames to enhance usability; or augmenting log and trace data with GeoIP information from the publicly available MaxMind geolocation database.

# Data Transformation

A robust telemetry pipeline may be expected to ingest a cornucopia of data types such as unstructured logs, structured logs in various formats, metrics time-series points, or events in the form of trace spans. In addition to being type-aware, the pipeline may provide APIs, either externally, or internally, in various wire formats. The functionality to transform those data types becomes a key component of a pipeline.

While it can be computationally expensive, the benefits of translating each data point into a common event format outweighs the costs associated with the compute needed to process the data. Such benefits include maximum flexibility around technology selection, minimal duplication of the same event in different formats, and the ability to further enrich the data.

Telemetry backends typically have discrete and unique APIs, and no standard pattern exists. Being able to support an external team’s needs for a particular backend can be valuable and as simple as writing a plug-in in a