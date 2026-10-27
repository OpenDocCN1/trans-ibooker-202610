# 11.2.6  Adding the correlation ID to the HTTP response with Spring Cloud Gateway

If we inspect the HTTP response from any service call made with Spring Cloud Sleuth, we’ll see that the trace ID used in the call is never returned in the HTTP response headers. If we inspect the documentation for Spring Cloud Sleuth, we’ll see that the Spring Cloud Sleuth team believes that returning any of the tracing data can be a potential security issue (though they don’t explicitly list their reasons why they believe this). But we’ve found that returning a correlation or tracing ID in the HTTP response is invaluable when debugging a problem.

Spring Cloud Sleuth lets us “decorate” the HTTP response information with its tracing and span IDs. The process to do this, however, involves writing three classes and injecting two custom Spring beans. If you’d like to take this approach, you can see it in the Spring Cloud Sleuth documentation here:

+   - https://cloud.spring.io/spring-cloud-static/spring-cloud-sleuth/1.0.12.RELEASE/

A much simpler solution is to write a Spring Cloud Gateway filter that injects the trace ID into the HTTP response. In chapter 8, where we introduced the Spring Cloud Gateway, we saw how to build a gateway response filter, adding the generated correlation ID to the HTTP response returned by the caller for use in our services. Now we’ll modify that filter to add a Spring Cloud Sleuth header. To set up our gateway response filter, we need to make sure that we have the Spring Cloud Sleuth dependencies in our pom.xml file like this:

```
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
```
```

We’ll use the spring-cloud-starter-sleuth dependency to tell Spring Cloud Sleuth that we want the gateway to participate in a Spring Cloud trace. Later in this chapter, when we introduce Zipkin, we’ll see that the gateway service will be the first call in any service invocation. Once the dependency is in place, the actual response filter is trivial to implement.

The following listing shows the source code used to build the filter. The file is located in /gatewayserver/src/main/java/com/optimagrowth/gateway/filters/ResponseFilter.java.

```
```java
Listing 11.5  Adding the Spring Cloud Sleuth trace ID via a response filter

package com.optimagrowth.gateway.filters;
// Imports removed for conciseness.
import brave.Tracer;
import reactor.core.publisher.Mono;

@Configuration
public class ResponseFilter {
```
```