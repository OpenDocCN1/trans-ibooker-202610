
platform to gain observability in your applications, underlying infrastructure, and networking stack.

## Introduction to Observability

The first thing to understand about observability is the word itself. The term *observability* originated in the world of control theory in mathematics, which primarily deals with creating a way to control *dynamic systems* using a controller with the help of feedback from the system itself. In control theory, observability is the ability to measure a system’s internal state from the knowledge of external outputs. With respect to software engineering, observability means knowing the *exact* state of an application from its outputs. By designing your application and infrastructure to be observable, you will essentially be able to track down *why* a problem is happening and *how* you can fix it. The underlying goal of making a system observable is to enable an engineer to clearly see a problem, from its cause to its effect.

Observability is even more relevant in today’s cloud native infrastructure because of the fast-paced, distributed environment where software and systems could eventually fail. The rise of microservices, containers, and serverless computing has resulted in a complicated mesh of interconnected services performing huge numbers of calls to each other. When these intertwined networks of services misbehave, it’s nearly impossible to find the root cause of the problem while avoiding disruption to the business. It becomes critical to have a fast way to pinpoint a problem in the underlying environment of complex services such that the business doesn’t suffer. Observability tackles this by letting you ask questions about your systems and quickly get answers.

Before we discuss how observability assists in modern distributed environments, let’s take a look at what constitutes an observable system, and discuss the principle behind the term and the philosophy behind the idea of building observability into systems.

## Observability: More Than Three Pillars

The fundamental idea behind observability is to gain a better understanding of an application through meaningful insights. As applications become more ephemeral and distributed, it becomes critical to define the guiding principles of an observable system. Cloud practitioners have defined *logs*, *metrics*, and *traces* as three major verticals, or pillars, that are foundational to building a modern observable system. Let’s take a quick look at these pillars and see why they are important.

### Metrics

Metrics represent numerical data measured over an interval of time. Metrics are usually collected by running an agent on a host which periodically sends the data to a central data store, and later aggregates the data to be represented over dashboards. Then the metrics are used to build alerting systems, since the metrics are stored in a time series database. An example of a metric is queries per second (qps).

### Logs

Logs represent the events that occur in a system (application and infrastructure). Logs contain details of internal state and events. Generally, they are written and stored on the application system in a structured format which can be parsed easily. A good logging architecture is the most fundamental tool for gaining most of the information in a system and is often the first debugging tool used by software developers. Following is an example of simple Apache server access logs:

```
10.185.248.71 - - [09/Jan/2015:19:12:06 +0000] 808840 "GET
/inventoryService/inventory/purchaseItem?
userId=20253471&itemId=23434300 HTTP/1.1" 500 17 "-" "Apache-
HttpClient/4.2.6 (java 1.5)"
```

### Traces

Last but not least, traces are the modern solution to the problem of learning how a single event is behaving in a distributed system. Traces help you identify the amount of processing (i.e., latency) being done at each layer of the stack. In the world of microservices and distributed systems, traces

provide valuable insights to events by creating a visual of correlated distributed calls between services.

Figure 6-1 showcases simple traces.

![](img/8d47d1b5dbca55e784ae602145a894c7_177_0.png)

Figure 6-1. A sample application showing traces

Just because you have built a system to include logs, metrics, and traces doesn’t necessarily mean the system is observable, however. All three pillars have their pros and cons, which we will discuss in the rest of the chapter, and individually, they are just the starting point since they enable you to look into a problem but they don’t directly address the use case or business needs of your company. To take advantage of the pillars you must use all of them together, along with the factors impacting your business. If you collect metrics data separately from logs and traces, you can easily lose context and end up observing only a single underlying system in isolation. Observability with respect to a unified approach means collecting and preserving the underlying context along with all its richness and dimensionality.

## Observability: A Superset of Monitoring

Monitoring primarily focuses on the passive collection of a system’s events, logs, and metrics. You generally monitor a critical service, which you think

might go wrong, because either you have experienced errors with the service before or you are aware of the service’s failure modes. Monitoring takes a proactive approach by assisting you with alerting and capacity planning, while observability takes a more reactive approach by enabling you to examine the application stack more closely to see what broke and learn why it broke.

Let’s look at an example. Imagine that a developer in your company changes a critical code path while trying to add a new feature in a flagship application. Unfortunately, he accidentally introduces a bug, which then causes an outage. In such a situation, your monitoring systems will be able to clearly show the increased rate of error from a service but will not be able to drill down further. However, having observability embedded in your stack can help you quickly drill down to the problem—say, an issue with a Redis Cluster—and then traces can lead you straight to the root cause.

It’s important to reiterate that there’s nothing wrong with your monitoring systems in this scenario. They are doing what they’re designed to do—tell you when something is wrong. Observability is what’s helping you figure out what went wrong.

It’s also important to note that observability is not an alternative to monitoring. Rather, it is a superset of monitoring because it provides both a high-level overview of services along with granular insights into various events inside a distributed system. Moreover, in a distributed cloud environment where multiple services run and interact with one another, the task of finding the fault becomes extremely painful if the system is not instrumented to expose the correct fault to bubble up. Observability handles such scenarios by pinpointing the problem chain as the request travels through the services.

## Observability-Driven Development

As we have discussed, cloud native applications are primarily distributed and microservices based, which makes them inherently complex to maintain and manage. Anyone who runs a production-grade application

also realizes that when applications start to misbehave, it becomes nearly impossible to figure out which particular moving part is the root cause of the misbehavior. Sometimes failures are caught with the help of traditional logging and monitoring setups, which can point you in the right direction given the complexity of your application stack. But as your services evolve, you cannot know with certainty all of the failure modes of your application or its underlying infrastructure.

Observability-driven development is an effort toward solving the problem of finding the unknowns in production environments by encouraging the addition of observability aspects early in the software development lifecycle. Moreover, it advocates for the instrumentation of application code during the software design phase so that it becomes easier to add instrumentation in one place rather than many places at a later stage. It also aims to bridge the gap between developers and the operational folks by letting the developers own the code and debug easily in production environments. Not only are the engineering teams able to diagnose faults and failures early in production, but it also helps these teams measure and analyze both operational and business performance.

In the following sections, we will look at various ways in which modern cloud native infrastructure and applications can be made observable. We will dive into each of the three pillars and see what is the preferred way of building your systems in cloud native environments, starting with metrics.

## Monitoring Metrics with Prometheus in a Cloud Native World

As we discussed in Chapter 4, Kubernetes has immensely changed how modern applications are created and deployed. Microservices and service-oriented architectures have transformed the landscape of cloud native environments as the speed of feature additions and deployments has increased. With the shift toward these modern architectures, the traditional approach of monitoring can’t keep up with the current pace of such architectures, which revolve around ephemeral infrastructure, multiple
