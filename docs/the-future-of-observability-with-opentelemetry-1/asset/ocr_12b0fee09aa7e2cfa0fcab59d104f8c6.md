# OpenTelemetry Control Plane

Currently, OpenTelemetry Collectors and SDKs are managed as independent units, which must be restarted to change their configurations. A control plane, which reports the current status of these components and allows for live configuration changes, is currently in development by the Agent Management SIG. It would allow an operator or an automated service to dynamically control processing across the entire telemetry pipeline.

Dynamic configuration will allow for fine-grained, responsive control over sampling, log levels, and other forms of resource management, thereby reducing costs. Eventually, this control plane may enable more advanced forms of tail-based sampling, sampling techniques that require coordination across the entire deployment.

# Columnar-Encoded OTLP

The current OpenTelemetry Protocol is an effective but straightforward encoding of the OpenTelemetry data model. Spans, metrics, and logs are encoded as...spans, metrics, and logs. This can be thought of as a row-based model, where every span, metric, or log is encoded as a discrete element in a list of elements. A column-based model would instead encode every element into a single table, where all elements share the same columns for overlapping concepts such as timestamps and attributes.

This columnar representation is expected to optimize the creation, size, and processing of data batches. The main benefits of a such an approach are:

+   - Better data compression rate (group of similar data)
+   - Faster data processing (better data locality means better use of the CPU cache lines)
+   - Faster serialization and deserialization (fewer objects to process)
+   - Faster batch creation (less memory allocation)
+   - Better I/O efficiency (less data to transmit)

The benefit of this approach increases proportionally with the size of the batches. Using the existing “row-oriented” representation is well suited for small batch scenarios. Therefore, columnar encoding would extend the current protocol. The current implementation