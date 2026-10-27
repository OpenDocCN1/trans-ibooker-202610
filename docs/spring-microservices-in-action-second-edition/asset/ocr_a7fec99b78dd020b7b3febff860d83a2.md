```
final Logger logger =LoggerFactory.getLogger(ResponseFilter.class);

@Autowire
Tracer tracer;

@Autowire
FilterUtils filterUtils;

@Bean
public GlobalFilter postGlobalFilter() {
    return (exchange, chain) -> {
        return chain.filter(exchange)
            .then(Mono.fromRunnable(() -> {
                String traceId =
                    Tracer.currentSpan()
                    .context()
                    .traceIdString();
                logger.debug("Adding the correlation id to the outbound headers. {}",traceId);
                exchange.getResponse().getHeaders()
                    .add(FilterUtils.CORRELATION_ID,traceId);
                logger.debug("Completing outgoing request for {}.",
                    exchange.getRequest().getURI());
            }));
    };
}
```

Sets the entry point to access trace and span ID information

Adds a span to the HTTP response header tmx-correlation-ID in the Spring Cloud Sleuth trace ID

Because the gateway is now Spring Cloud Sleuth–enabled, we can access tracing information from within our ResponseFilter by autowiring in the Tracer class. This class lets us access information about the currently executed trace. The tracer.currentSpan().context().traceIdString() method enables us to retrieve the current trace ID as a String for the transaction underway. It’s trivial to add the trace ID to the outgoing HTTP response passing back through the gateway. This is done with the following method call:

```
exchange.getResponse().getHeaders()
    .add(FilterUtils.CORRELATION_ID, traceId);
```

With this code in place, if we invoke an O-stock microservice through our gateway, we should get an HTTP response called tmx-correlation-id with a Spring Cloud Sleuth trace ID. Figure 11.14 shows the results of a call to the following endpoint to GET the licenses of an organization:

```
http://localhost:8072/license/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c7/license/4af05c3b-a0f3-411d-b5ff-892c62710e14
```