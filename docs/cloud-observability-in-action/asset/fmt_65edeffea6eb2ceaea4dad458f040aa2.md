
One advantage of the waterfall visualization is that we can tell, simply by visually inspecting all the spans in the trace, where most of the time is spent (in our case, in figure 8.3, the checkout service spends around half of the time in the billing phase). With this concrete motivational example of a microservices-based e-commerce app established, let’s move on to a more formal definition of terms you will come across in the distributed tracing space.

### 8.1.2 Terminology

While there are differences in terminology across vendors, the following are terms that are usually well defined and understood across offerings. I’m following mostly the `OpenTelemetry` nomenclature ([http://mng.bz/d1QN](http://mng.bz/d1QN)), especially where there is no consensus among distributed tracing solutions:

*   A `span` represents a unit of work and, with it, captures a part of the execution of the request (compare with figure 8.2). This can happen within the scope of a service, such as the execution of a function in a service or across services. A special kind of span in a trace is the `root span`, which is created when the request enters the app. A span has an ID, and typically, it stores the ID of the trace it belongs to and the root span it was triggered from as well as payload-including metadata (think timestamps, tags, etc.). Note that in AWS X-Ray, this unit is called a segment.

*   A `trace` is the end-to-end processing of a request in a distributed system, consisting of a group of spans that are temporally ordered. It has an ID and metadata.

*   In the context of cross-service calls, the `trace context` captures all unique identifiers for individual traces and requests. This enables a trace to exist beyond a single service via a process called `context propagation`, where this trace context is forwarded in a network call. The calling service (parent) attaches all necessary data for the trace context to the request to the downstream (child) service, such as was the case in our example e-commerce app (figure 8.2) between the billing service and the third-party payment provider. Nowadays, with `OpenTelemetry` establishing itself as the standard, this propagation is usually done based on the `Trace Context` ([https://www.w3.org/TR/trace-context/](https://www.w3.org/TR/trace-context/)) W3C Recommendation (the standard way to use HTTP headers to propagate the context).

*   To reduce the volume of data (number of traces and spans), you can make use of a technique called `sampling`. This allows you to selectively drop traces based on a characteristic including a fixed percentage or based on the nature of a trace—for example, if an error is present in one of the services, and with it we see that error fagged in the span representing the service.

Now that we have a basic understanding of distributed tracing terms and what they mean, let’s look at some use cases.

### 8.1.3 Use cases

Distributed tracing can be useful not just in distributed systems where services call others over the network but also for monolithic apps. In general, you can benefit from it in the following cases:

*   *Monitoring microservices-based applications*—These allow you to acquire an end-to-end overview of the actual architecture of your app, including call dependencies. Rather than relying on static architecture diagrams of how the app is supposed to work, you get a visual representation of how the app actually works. You can determine error states (what services error out) and performance issues (latency graphs of service invocations) of services, usually with a single glance at a service map.

*   *Optimization*—This is used to determine where along the request path most of the time is spent. You can pinpoint the slowest service and start the latency optimization there, promising the most yield in terms of investment.

*   *Release management*—When rolling out a new version of a service, you can assess the impact on the overall request paths the service is part of, comparing before and after the new version.

*   *Troubleshooting*—In the case of an incident, as we discussed in chapter 7, you can use the distributed tracing solution to figure out which services contribute to the issue and can roll them back to a known good state to stop the bleeding. We will see this common use case in action in the next subsection of this chapter.

OK, that was more than enough theory! Let’s get our hands dirty with distributed tracing, focusing on the troubleshooting use case.

## 8.2 Using distributed tracing in a microservices app

In this section, you will use distributed tracing in the context of a simple example microservices app. The goal is to understand how the services work together and, once we start simulating errors, use distributed tracing to identify which services cause issues and what kind of errors you encounter.

### 8.2.1 Example app overview

Our example app (to be set up later) consists of four services, as depicted in figure 8.4. We have a frontend service as the entry point, which calls two services: service `A` and service `B`; in turn, service `B` calls two other services (`B0` and `B1`). Some of the services also call out to static assets on the web to simulate calls to external third-party APIs. Note that our example app doesn’t do anything useful; all of the services use the `ho11y` ([http://mng.bz/rWxg](http://mng.bz/rWxg)) synthetic signal generator to generate traces, and we can configure it to simulate certain failure modes.

![](OEBPS/Images/08-04.png)

Figure 8.4 Example microservices app architecture

Our example app stack contains the five services making up the microservices app as well as the `OpenTelemetry` collector to ingest the spans generated from the four services into the distributed tracing tool (`CNCF Jaeger`). The setup is based on what you already saw in chapter 6.

### 8.2.2 Implementing the example app

To stand up our example app stack, using the Docker Compose file shown in listing 8.1, execute the following command from within the `ch08/jaeger/` directory:

```
docker-compose -f docker-compose.yaml up
```

Note The `ho11y` synthetic signal generator has several configuration options that are defined via environment variables. With `HO11Y_PORT`, you can define on which port `ho11y` should be listening for service requests, and with `DOWNSTREAM0` to `DOWNSTREAM4`, you can define URLs that the `ho11y` instance should call (and, hence, allow you to wire up and simulate arbitrary complex microservices). To simulate dropping requests, we will use the `HO11Y_INJECT_ FAILURE` environment variable, which causes the `ho11y` instance to return some `4xx` or `5xx` HTTP status codes in half of the cases, rather than returning a 200 HTTP status code. Finally, to simulate another kind failure mode (throttling of the upstream service), we will use the `HO11Y_CUTOFF_TPS` environment variable to define a cutoff point, beyond which `ho11y` will return a 429 HTTP status code ([https://httpstatuscodes.org/429/](https://httpstatuscodes.org/429/)).

Listing 8.1 shows the relevant snippets (focusing on the wiring and configuration of our microservices app) of the Docker Compose file.

Listing 8.1 The Docker Compose configuration for the Jaeger setup
