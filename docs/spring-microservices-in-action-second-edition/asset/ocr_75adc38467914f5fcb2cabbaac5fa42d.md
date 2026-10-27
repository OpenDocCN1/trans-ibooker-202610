headers that are not part of the HTTP specification and **B3**, which stands for “Big-BrotherBird,” Zipkin’s previous name.

> **NOTE**  If you want to know more about the MDC fields, we highly recommend that you read the SL4J logger documentation at http://www.slf4j.org/manual.html#mdc and the Spring Cloud Sleuth documentation at https://cloud.spring.io/spring-cloud-static/spring-cloud-sleuth/2.1.0.RELEASE/single/spring-cloud-sleuth.html.

You can also configure the log data shown in figure 11.5 by using the Logging-EventCompositeJsonEncoder. Using this composite encoder, we can disable all the providers that were added by default to our configuration, add new patterns to display custom or existent MDC fields, and more. The following listing shows a brief example of the logback-spring.xml configuration that deletes some output fields and creates a new pattern with a custom field and other existent fields.

```
```xml
<encoder class="net.logstash.logback.encoder
                 .LoggingEventCompositeJsonEncoder">
  <providers>
    <mdc>
      <excludeMdcKeyName>X-B3-TraceId</excludeMdcKeyName>
      <excludeMdcKeyName>X-B3-SpanId</excludeMdcKeyName>
      <excludeMdcKeyName>X-B3-ParentSpanId</excludeMdcKeyName>
    </mdc>
    <context/>
    <version/>
    <logLevel/>
    <loggerName/>
    <pattern>
      <pattern>
        <omitEmptyFields>true</omitEmptyFields>
        {
          "application": {
            "version": "1.0"
          },
          "trace": {
            "trace_id": "%mdc{traceId}",
            "span_id": "%mdc{spanId}",
            "parent_span_id": "%mdc{X-B3-ParentSpanId}",
            "exportable": "%mdc{spanExportable}"
          }
        }
      </pattern>
    </pattern>
    <threadName/>
    <message/>
    <logstashMarkers/>
    <arguments/>
    <stackTrace/>
  </providers>
</encoder>
```
```