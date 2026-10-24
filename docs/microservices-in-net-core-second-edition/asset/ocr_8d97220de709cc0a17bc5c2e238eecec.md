same is true for the logging work you have done in this chapter. It is the same for all the microservices. In chapter 12, you'll extract the common pieces of code from the shopping cart and package them in NuGet packages, ready to be reused across many microservices.

# Summary

+   - Microservice systems need monitoring and logging just like any other server-side systems.
+   - Every microservice should be monitored and sending log messages.
+   - Because of the number of microservices in a system, setting up monitoring and logging needs to be easy.
+   - Trace IDs make it easier to trace a request across several microservices.
+   - ASP.NET implements the trace context standard and therefore uses the `traceparent` HTTP header to propagate trace IDs.
+   - You can use ASP.NET’s health check middleware to create two monitoring endpoints: one for startup and one for liveness.
+   - Structured logging is a good way to include valuable information in log messages and make that data searchable.
+   - Serilog is a library for doing structured logging.
+   - ASP.NET stores trace information in `Activity.Current`.
+   - You can configure Serilog to include trace information from `Activity.Current` in all log messages
+   - The `HttpClient` will include a `traceparent` header with every outgoing HTTP request, making sure trace IDs are sent to other microservices.
+   - Kubernetes supports health probes that fit well with our monitoring endpoints.
+   - Kubernetes allows us to easily access the logs from our containers.
+   - To get the full benefit of the logs all log messages should be shipped to a central logging microservice with good search and visualization capabilities.