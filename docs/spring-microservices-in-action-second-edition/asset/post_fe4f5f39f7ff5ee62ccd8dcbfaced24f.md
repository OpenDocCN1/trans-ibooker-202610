
## CHAPTER 8 *Service routing with Spring Cloud Gateway*

- *Post-filters*—A post-filter is invoked after the target service, and a response is sent back to the client. Usually, we implement a post-filter to log the response back from the target service, handle errors, or audit the response for sensitive information.

![](img/3c6dc613426280ca3eafe43dea24391e_255_0.png)

**Figure 8.10** The pre-filters, target route, and post-filters form a pipeline in which a client request flows. As a request comes into the gateway, custom filters can manipulate the incoming request.

If we follow the flow laid out in figure 8.10, everything starts with a service client making a call to a service that’s exposed through the service gateway. From there, the following takes place:

1. Any pre-filters defined in the gateway are invoked as a request enters the gateway. The pre-filters inspect and modify an HTTP request before it gets to the actual service. A pre-filter, however, cannot redirect the user to a different endpoint or service.
2. After the pre-filters are executed against the incoming request by the gateway, the gateway determines the destination (where the service is heading).
3. After the target service is invoked, the gateway post-filters are invoked. The post-filters inspect and modify the response from the invoked service.

The best way to understand how to implement the gateway filters is to see them in action. To this end, in the next several sections, we’ll build pre- and post-filters and then run client requests through them. Figure 8.11 shows how these filters will fit together in processing requests for our O-stock services.

![](img/3c6dc613426280ca3eafe43dea24391e_256_0.png)

Figure 8.11 Gateway filters provide centralized tracking of service calls and logging. These filters allow us to enforce custom rules and policies against microservice calls.

Following the flow in figure 8.11, we can see the following custom filters in use:

- *Tracking filter*—The tracking filter is a pre-filter that ensures that every request flowing from the gateway has a correlation ID associated with it. A *correlation ID* is a unique ID that gets carried across all the microservices that are executed when carrying out a customer request. A correlation ID allows us to trace the chain of events that occur as a call goes through a series of microservice calls.
- *Target service*—The target service can either be an organization or the licensing service. Both services receive the correlation ID in the HTTP request header.
- *Response filter*—The response filter is a post-filter that injects the correlation ID associated with the service call into the HTTP response header sent to the client. This way, the client will have access to the correlation ID associated with the request.

### 8.5 Building the pre-filter

Building filters in the Spring Cloud Gateway is straightforward. To begin, we’ll build a pre-filter, called `TrackingFilter`, that will inspect all incoming requests to the gateway and determine whether there’s an HTTP header called `tmx-correlation-id` present in the request. The `tmx-correlation-id` header will contain a unique GUID (Globally Unique Identifier) that can be used to track a user’s request across multiple microservices:

- If the `tmx-correlation-id` isn’t present on the HTTP header, our gateway `TrackingFilter` will generate and set the correlation ID.
- If there’s already a correlation ID present, the gateway won’t do anything. (The presence of a correlation ID means that this particular service call is part of a chain of service calls carrying out the user’s request.)

> **NOTE** We discussed the concept of a correlation ID in chapter 7. Here, we’ll walk through how to use Spring Cloud Gateway to generate a correlation ID in more detail. If you skipped around in the book, we highly recommend you look at chapter 7 and read the section on thread context. Our implementation of correlation IDs will be implemented using `ThreadLocal` variables, and there’s extra work to do in order to have your `ThreadLocal` variables work.

Let’s go ahead and look at the implementation of the `TrackingFilter` in listing 8.8. You can also find this code in the book samples directories in the class file `/gatewayserver/src/main/java/com/optimagrowth/gateway/filters/TrackingFilter.java.`

**Listing 8.8 Pre-filter for generating correlation IDs**

```java
package com.optimagrowth.gateway.filters;
//Other imports omitted for conciseness

import org.springframework.http.HttpHeaders;
import reactor.core.publisher.Mono;

@Order(1)
@Component
public class TrackingFilter
                implements GlobalFilter {

    private static final Logger logger =
                LoggerFactory.getLogger(TrackingFilter.class);

    @Autowired
    FilterUtils filterUtils;

    @Override
    public Mono<Void> filter(ServerWebExchange exchange,
                GatewayFilterChain chain) {
        HttpHeaders requestHeaders =
                exchange.getRequest().getHeaders();
        if (isCorrelationIdPresent(requestHeaders)) {
            logger.debug(
                "tmx-correlation-id found in tracking filter: {}.",
                filterUtils.getCorrelationId(requestHeaders));
        } else {
            String correlationID = generateCorrelationId();
            exchange = filterUtils.setCorrelationId(exchange,
                    correlationID);
            logger.debug(
                "tmx-correlation-id generated in tracking filter: {}.",
                correlationID);
        }
        return chain.filter(exchange);
    }

    private boolean isCorrelationIdPresent(HttpHeaders
                requestHeaders) {
        if (filterUtils.getCorrelationId(requestHeaders) != null) {
            return true;
        } else {
            return false;
        }
    }

    private String generateCorrelationId() {
        return java.util.UUID.randomUUID().toString();
    }
}
```

To create a global filter in the Spring Cloud Gateway, we need to implement the GlobalFilter class and then override the filter() method. This method contains the business logic that the filter implements. Another critical point to note from the previous code is the way that we obtain the HTTP headers from the ServerWebExchange object:

```java
HttpHeaders requestHeaders = exchange.getRequest().getHeaders();
```

![](img/3c6dc613426280ca3eafe43dea24391e_258_0.png)

We’ve implemented a class called FilterUtils, which encapsulates common functionality used by all the filters. It’s located in the /gatewayserver/src/main/java/com/optimagrowth/gateway/filters/FilterUtils.java file. We’re not going to walk through the entire FilterUtils class, but we’ll discuss several key methods: getCorrelationId() and setCorrelationId(). The following listing shows the code for the FilterUtils getCorrelationId() method.

**Listing 8.9 Retrieving the tmx-correlation-id with getCorrelationId**

```java
public String getCorrelationId(HttpHeaders requestHeaders){
    if (requestHeaders.get(CORRELATION_ID) !=null) {
        List<String> header = requestHeaders.get(CORRELATION_ID);
        return header.stream().findFirst().get();
    } else{
        return null;
    }
}
```

The key thing to notice in listing 8.9 is that we first check to see if tmx-correlation-ID is already set on the HTTP headers for the incoming request. If it isn’t there, our code should return null to create one later on. You may remember that earlier, in the filter() method in our TrackingFilter class, we did exactly this with the following code:

```java
} else {
    String correlationID = generateCorrelationId();
    exchange = filterUtils.setCorrelationId(exchange, correlationID);
    logger.debug("tmx-correlation-id generated in tracking filter: {}",
                correlationID);
}
```

To set tmx-correlation-id, you’ll use the FilterUtils setCorrelationId() method as shown in the following listing.

**Listing 8.10 Setting the tmx-correlation-id in the HTTP headers**

```java
public ServerWebExchange setRequestHeader(ServerWebExchange exchange,
                                        String name, String value) {
    return exchange.mutate().request(
        exchange.getRequest().mutate()
            .header(name, value)
            .build())
        .build();
}

public ServerWebExchange setCorrelationId(ServerWebExchange exchange,
                           String correlationId) {
    return this.setRequestHeader(exchange,CORRELATION_ID,correlationId);
}
```
