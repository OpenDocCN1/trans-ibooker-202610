generated SpanEvents are sent to Wallace over HTTP as JSON or Protobuf-encoded events.

Wallace, based on Murron, is a receiver and processor that operates independently from our core cloud infrastructure, so we can capture client and service errors even when Slack’s core infrastructure is experiencing a full site outage. Wallace validates the span data it receives and forwards those events to another Murron receiver that writes the data to our buffer, Kafka.

Our internal Java and Go applications use the open source instrumentation libraries from Zipkin and Jaeger, respectively. To capture the spans from these applications, Wallace exposes receivers for both types of span data. These receivers, called *trace adapters*, translate the reported spans into our SpanEvent format and write them to Wallace, which in turn forwards them to a Murron receiver that writes the data to Kafka.

The trace data written to Kafka is consumed primarily by three exporters based on Murron and Secor, which is an open source project maintained by Pinterest:

# *Elasticsearch exporter*

A Murron exporter reads our SpanEvents from Kafka and sends them to Elasticsearch, an event store that allows us to display events on a dashboard (in our case, we primarily use Grafana) and set custom alerts on those events.

# *Data warehouse exporter*

We use Secor, an open source project by Pinterest, to transform the data from Kafka and upload it to Amazon S3 to be ingested into our Presto data warehouse.

# *Honeycomb exporter*