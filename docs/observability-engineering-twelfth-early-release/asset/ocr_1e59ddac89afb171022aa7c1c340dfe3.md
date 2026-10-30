# A CRASH-COURSE IN OPENTELEMETRY CONCEPTS

OpenTelemetry provides the libraries, agents, tooling, and other components designed for the creation and management of telemetry data from your services. OTel brings together APIs for different telemetry data types and manages the distributed context propagation common to all of them. It combines concepts and components from each of its predecessor projects to retain their advantages while still being backward compatible with previously written instrumentation.

To do this, OTel breaks functionality into several components with a distinct set of terminology. Useful concepts to keep in mind with OTel are as follows:

*API*

The specification portion of OTel libraries that allows developers to add instrumentation to their code without concern for the underlying implementation.

*SDK*

The concrete implementation component of OTel that tracks state and batches data for transmission.

*Tracer*

A component within the SDK that is responsible for tracking which span is currently active in your process. It also allows you to access and modify the current span to perform operations like adding attributes, events, or finishing it when the work it tracks is complete.

*Meter*