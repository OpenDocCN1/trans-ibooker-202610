![](bbox=[0.125, 0.075, 0.875, 0.235])

Figure 11.18 Viewing the details of a trace that shows how a licensing service call flows from the gateway to the licensing service and then to the organization service.

# 11.3.7 Capturing messaging traces

Messaging can introduce its own performance and latency issues inside an application. A service might not be processing a message from a queue quickly enough. Or there could be a network latency problem. We’ve encountered these scenarios while building microservice-based applications.

Spring Cloud Sleuth sends Zipkin trace data on any inbound or outbound message channel registered in the service. Using Spring Cloud Sleuth and Zipkin, we can identify when a message is published from a queue and when it’s received. We can also see what behavior takes place when the message is received on a queue and processed. And, you’ll remember from chapter 10, when an organization record is added, updated, or deleted, a Kafka message is produced and published via Spring Cloud Stream. The licensing service receives the message and updates a Redis key-value store that it uses to cache the data.

Next we’ll delete an organization record and watch Spring Cloud Sleuth and Zipkin trace the transaction. We’ll issue a DELETE to the following endpoint for the organization service via Postman:

```
http://localhost:8072/organization/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c7
```

Earlier in the chapter, we saw how to add the trace ID as an HTTP response header, adding a new HTTP response header called tmx-correlation-id. We had the tmx-correlation-id returned on our call with a value of 054accff01c9ba6b. We can search Zipkin for this specific trace by entering the trace ID returned by our call into the search box in the upper-right corner of the Zipkin query screen. Figure 11.19 shows where we can enter the trace ID.

With the specific trace in hand, we can query Zipkin for the transaction and view the publication of the DELETE message. The second span in figure 11.20 is the output message from the channel output, which is used to publish to a Kafka topic called orgChangeTopic. Figure 11.20 shows the output message channel and how it appears in the Zipkin trace.