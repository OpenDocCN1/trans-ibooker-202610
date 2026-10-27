![](bbox=[0.192, 0.081, 0.871, 0.236])

**Figure 11.19** With the trace ID returned in the HTTP `tmx-correlation-id` response header, we can easily find the transaction by searching on the specific trace ID.

![](bbox=[0.192, 0.329, 0.871, 0.582])

**Figure 11.20** Spring Cloud Sleuth automatically traces the publication and receipt of messages on the Spring message channels. We can view the details of the trace with Zipkin.

You can see the licensing service receive the message by clicking the licensing service span. Figure 11.21 shows this data.

Until now, we’ve used Zipkin to trace our HTTP and messaging calls from within our services. However, what if we want to perform traces out to third-party services that aren’t monitored by Zipkin? For example, what if we want to get tracing and timing information for a specific Redis or Postgres SQL call? Fortunately, Spring Cloud Sleuth and Zipkin allow us to add custom spans to our transactions so that we can trace the execution time associated with third-party calls.