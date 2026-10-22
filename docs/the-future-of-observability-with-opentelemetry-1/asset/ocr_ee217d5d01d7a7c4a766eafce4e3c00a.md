# Propagators

In order for distributed tracing to work, the trace context must be shared by every service that participates in the transaction. Propagators accomplish this by serializing and deserializing the context object, allowing the signals to follow their transactions across network requests.

# Tracing

The OpenTelemetry tracing system is based on OpenTracing and OpenCensus. Both of these systems, along with the popular Zipkin and Jaeger projects, are based on the Dapper tracing system developed at Google. OpenTelemetry seeks to be compatible with all of these Dapper-based systems.

OpenTelemetry tracing includes a concept called links, which allow separate traces to be combined into a larger graph. This is used to connect transactions to background processing and to observe large asynchronous systems such as Kafka and AMQP.

# Metrics

Metrics is a big space, containing a wide variety of approaches and implementations. The OpenTelemetry metrics signal is designed to be fully compatible with Prometheus and StatsD.

Metrics include trace exemplars, automatically associating metrics with a sampling of the traces that generated them. Associating metrics and traces by hand is often a tedious and error-prone task, and performing this task automatically will save operators a significant amount of time.

# Logs

OpenTelemetry combines a highly structured logging API with a high-speed log-processing system. Existing logging APIs can be connected to OpenTelemetry, preventing the need to re-instrument an application.

Whenever it is present, logs are automatically attached to the current trace. This makes transaction logs easy to find and allows automated analysis to find accurate correlations between logs in the same trace.