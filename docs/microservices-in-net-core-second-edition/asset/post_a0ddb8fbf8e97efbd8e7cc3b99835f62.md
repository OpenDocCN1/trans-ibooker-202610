
Figure 9.2 Each microservice is monitored by polling an endpoint.

![](img/608267d7803dc57a1a896ad121f37868_230_0.png)

Figure 9.3 Every microservice should have two monitoring endpoints: one at /health/startup and one at /health/live. The monitoring system polls both endpoints. As long as both respond with a success response, the monitor considers the microservice to be up.

![](img/608267d7803dc57a1a896ad121f37868_230_1.png)

When everything is okay the endpoint responds with a simple 200 OK response:

```
HTTP/1.1 200 OK
```

The second endpoint—at /health/live—checks the internal health of the microservice and, if everything is okay, also responds with a 200 OK status code. What checking the internal health of a microservice entails differs from microservice to microservice.

You can implement both monitoring endpoints using the health check middleware that comes with ASP.NET, as illustrated in figure 9.4.

![](img/608267d7803dc57a1a896ad121f37868_231_0.png)

Figure 9.4 You can use ASP.NET's health check middleware to implement monitoring endpoints.

When a request to the /health/startup endpoint comes in, the health check middleware gives a 200 OK response if the microservice is ready to receive requests. When a request to the /health/live endpoint comes in, the health check middleware runs a health check and then responds with 200 OK if the check succeeded. Other requests pass through the monitoring middleware and onto the rest of the pipeline endpoint up in the MVC.

ASP.NET has a built-in health check middleware we can extend with health checks specific to our microservices to implement the two monitoring endpoints. Later in this chapter, we will implement health checks and configure Kubernetes to use them to monitor our microservice.

## 9.2 **Logging needs in microservices**

In addition to monitoring each individual microservice, you also need to send out log messages regarding failures, errors, performance, and whatever else you need insight into. As discussed in chapter 7, you can introduce a centralized logging microservice that receives log messages from all the other microservices, saves them (e.g., in a search engine), and provides easy access to them through dashboards and search UIs (see figure 9.5). Again, you want to be able to easily set up each microservice to send log messages to the logging microservice.

![](img/608267d7803dc57a1a896ad121f37868_232_0.png)

**Figure 9.5** All microservices send log messages to the central logging microservice.

Propagating trace IDs throughout the system and including them in log messages enable us to follow—or trace—requests through the system. This provides context, which allows us to better troubleshoot issues.

The trace ID is sent from one microservice to another by using the traceparent header, as specified in the W3C trace context standard,¹ which specifies four fields that together form the traceparent header: version, trace ID, parent ID, and trace flags. The trace ID field is the one we are primarily interested in because it is meant to follow the request all the way through the system. The parent ID provides more local context and is allowed to change as the request moves through the system. The format of the traceparent header is:

```
"version"-"trace id"-"parent id"-"trace flags"
```

For instance, if the trace ID is cb32e1c31b369748bf45958f8a49e3e7 and the parent ID is 90757069a8f0ea4b, the trace parent header looks like this:

![](img/608267d7803dc57a1a896ad121f37868_234_0.png)

This means a request from the shopping cart microservice to the product catalog microservice could look like this:

```
HTTP GET /products?productIds=[1,2] HTTP/1.1
Accept: application/json
traceparent: 00-cb32e1c31b369748bf45958f8a49e3e7-90757069a8f0ea4b-00
```

This will tell the product catalog microservice that the trace ID is cb32e1c31b369748bf45958f8a49e3e7 and that trace ID should be included in all log messages.

On top of propagating trace IDs in the HTTP requests, we should include them in events. Saving the trace ID along with events when the event is raised allows us to trace why an event was raised after the fact. This, again, is a strong troubleshooting tool.

ASP.NET implements the trace context standard, so out of the box ASP.NET will look for traceparent headers on incoming requests and propagate the trace ID to outgoing requests made with HttpClient. If there is no traceparent header on incoming requests, ASP.NET will assign a random one. We will see this in action shortly.

### 9.2.2 Structured logging with Serilog

Traditional logging libraries treat log messages as simple strings, possibly with an exception attached, but we’ll use something better: a structured logging library named

Serilog. The idea of structured logging is to allow log messages to contain structured data: a log message contains not only a flat message—like something went wrong—but also objects.

Serilog introduces a bit of syntax to log messages on top of .NET format strings. The extra syntax lets you give names to parameters and control whether a parameter’s value should be converted to a string with a .ToString() call or whether the object should be included as a whole in the log message. Named parameters are enclosed in braces, for example, "{RequestTime}". You tell Serilog to include the entire value in the message by putting an @ in front of the name: "{@Request}". As with .NET format strings, you add formatting directives to parameters without a leading @.

For instance, you can use Serilog in conjunction with .NET’s own logger to send a log message like this—assuming that log is an instance the .NET type ILogger configured to use Serilog:

```
var simplifiedRequest = new
{
    Path = "/foo",
    Method = "GET",
    Protocol = "HTTP",
};

var requestTime = 200;

log.LogInformation(
    "Processed request: {@Request} in {RequestTime:000} ms.",
    simplifiedRequest,
    requestTime);
```

simplifiedRequest and requestTime are objects that are part of the message. Like other logging frameworks, Serilog lets you configure different sinks to which log messages are written. The sink you’ll use here is the console sink. When the previous log message is written to the console, simplifiedRequest and requestTime are JSON serialized and inserted into the log message string. Serilog also adds some metadata, so the message written to the console looks like this:

```
2020-12-03 09:14:33 INF Processed request {"Path":"/foo", "Method":"GET", "Protocol":"HTTP"} in 200 ms.
```

As you can see, this preserves all the data from the original log message, because the complete simplifiedRequest object and requestTime are included in the message written to the console. This is nice, but it becomes even better when we ship the logs to a search engine like Elasticsearch. When you do that, the objects included in the log messages—like simplifiedRequest—are stored and indexed in the search engine. This means you can search for log messages based on values of particular properties of the objects included in the messages. For instance, you might search for log messages with a Request object that has a Path property with the value /foo. The previous message would be included, but a log message about a request to /bar wouldn’t.

Preserving the structure of the data included in log messages means the web frontend of the logging microservice can present the log message in a structured way. The end results are logs that are easier to search and get an overview from, include more data, and that you can drill into as needed. One such frontend is Kibana, which works well with logs stored in Elasticsearch and provides powerful search and visualization tools.

As you’ll see when you implement the logging later, you can configure Serilog to include extra data with every log message. One example of such extra information is the tracing information from the transparent header covered in the previous section. Including that in every log message enables you to efficiently search for all log messages with a given trace ID. Since the trace IDs are propagated across services, this allows us to follow—or indeed, trace—the log messages related to a particular operation as they move through the system (e.g., as illustrated in figure 9.6, when a customer adds an item to a shopping cart and there is an incoming request to the API Gateway, which is assigned a trace ID). The trace ID follows the “add to shopping cart” operation as it moves to other services—the shopping cart, the product catalog, and the price calculation microservices. By searching for log messages with the trace ID assigned to that “add to shopping cart” operation, you can see all log information related to that particular instance of adding items to the shopping cart. You can see what the API Gateway did, what the shopping cart did, what the product catalog did, and what the price calculation microservice did. This is very powerful when debugging production issues.

For the remainder of this chapter, you’ll be implementing monitoring and logging. You want to do this in all microservices, so you can work in the context of any one of the microservices you’ve already implemented. I’ll use the shopping cart microservice as an example and build on top of the code from chapter 7, but the code fits just the same in the other microservices.

## 9.3 Implementing the monitoring endpoints

In this section, you’ll use ASP.NET’s *health checks* support to implement the two monitoring endpoints we discussed earlier. The endpoints will behave as follows:

- `/health/live` responds to every request with a `200 OK` status, to indicate that the microservice is running and able to handle requests. Depending on the implementation of your microservices, you may want to add a deeper health check in this endpoint.
- `/health/startup` performs a basic health check and then responds with `200 OK` status if the health check succeeds. If the health check fails, it responds with `503 Service Unavailable`. The health check will query the shopping cart microservice database to check that a connection to the database can be established because the shopping cart microservice is ready to receive requests as soon as it is able to query its database.
