A component within the SDK that is responsible for tracking which metrics are available to report on in your process. It also allows you to access and modify the current metrics to perform operations like adding values, or retrieving those values at periodic intervals.

# Context propagation

An integral part of the SDK that deserializes context about the current inbound request from headers such as W3C `TraceContext` or B3M, tracks what the current request context is within the process, and serializes it to pass downstream to a new service.

# Exporter

A plug-in for the SDK that translates OTel in-memory objects into the appropriate format for delivery to a specific destination.
Destinations may be local (such as a log file or stdout) or may be remote (backends can be open source, such as Jaeger, or proprietary commercial solutions, like Honeycomb or Lightstep). For remote destinations, OTel defaults to using an exporter speaking the wire protocol known as OpenTelemetry Protocol (OTLP), which can be used to send data to the OpenTelemetry Collector.

# Collector

A standalone binary process that can be run as a proxy or sidecar that receives telemetry data (by default in OTLP format), processes it, and tees it to one or more configured destinations.

A deep dive into OTel components is beyond the scope of this book, but you can find out more about its concepts by reading the