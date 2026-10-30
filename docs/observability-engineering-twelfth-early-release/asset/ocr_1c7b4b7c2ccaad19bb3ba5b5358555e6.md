[OpenTelemetry documentation](OpenTelemetry documentation).

# Start with Automatic Instrumentation

One of the largest challenges of adopting distributed tracing is getting enough useful data to be able to map it to the knowledge you already have about your systems. How can you teach your observability systems about the services, endpoints, and dependencies you have so that you can start gaining insight? While you *could* manually start and end trace spans for each request, there’s a better way to decrease the friction and get insight in hours or days, not months.

For this purpose, OTel includes automatic instrumentation to minimize the time to first value for users. Because OTEL’s charter is to ease adoption of the cloud native ecosystem and microservices, it supports the most common frameworks for interactions between services. For example, OTEL automatically generates trace spans for incoming and outgoing gRPC, HTTP, and database/cache calls from instrumented services. This will provide you with at least the skeleton of who calls whom in the tangled web of microservices and downstream dependencies.

To provide you with that automatic instrumentation of request properties and timings, the framework needs to call OTEL before and after handling each request. Thus, common frameworks often support wrappers, interceptors, or middleware that OTEL can hook into in order to automatically read context propagation metadata and create spans for each request. In Go, this configuration is explicit because Go requires explicit type safety and compile-time configuration; however, in languages such as Java and .NET, you can attach a standalone OpenTelemetry Agent at runtime that will infer which frameworks are running and automatically register itself.

To illustrate the wrapper pattern, in Go, the common denominator for Go is the `http.Request/http.Response HandlerFunc` interface. You