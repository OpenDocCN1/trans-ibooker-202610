to be as performant as possible, extensibility and performance always come as a trade-off. For example, creating a binding to the OpenTelemetry C++ SDK via foreign function interface has the potential to be a very efficient option for dynamic scripting languages such as Ruby, Python, and Node.js.

There are also streaming architectures that show promising performance boosts. With many of these optimized solutions, the ability to write plug-ins and lifecycle hooks would be severely limited; the data structures needed to support those types of features would be nonexistent in these optimized solutions. Ultimately, there is no “perfect implementation”; there are only trade-offs.

The API/SDK separation is a key design choice, which the project makes heavy use of. For example, besides the SDK, every language has a no-op implementation, which is installed by default. There also is a Fake/Mock implementation available, which we use for testing. And there is the potential for even more creative implementations—for example, building developer tools for distributed systems, such as a live debugger, which works across network boundaries.

# Client Architecture: The SDK

The OpenTelemetry project provides an official implementation for the OpenTelemetry API, which we call the OpenTelemetry SDK. The SDK implements the OpenTelemetry API by providing a plug-in framework. The tracing SDK is described below; a similar architecture is applied to metrics and logs.

The basic data structure is a lock-free SpanData object. The SpanData object is created when a user starts a span, and it is built up automatically as the user adds attributes and events. Once a span is ended, the SpanData object will no longer be updated and can be safely passed to a background thread.

The SDK plug-in architecture is organized as a pipeline. For tracing, that pipeline consists of a chain of SpanProcessors. Each processor gets synchronous access to the SpanData object twice: once when the span starts and again after it has ended. Samplers, log appenders, and data scrubbers are examples of SpanProcessors. The last processor in the chain is often a BatchSpanProcessor, which manages a buffer of completed spans. An exporter can be connected to the