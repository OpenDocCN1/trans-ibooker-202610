
| Role | Description |
|---|---|
| DevOps | Uses metrics to narrow down components causing high latency; then jumps to traces to drill into a specific request path; and then looks at logs of one microservice, serving on that request path for details (correlation) |
| SRE | Tracks goals and their fulfillment, advising developers via dashboards |
| Platform operator | Uses a cluster-level dashboard to track autoscaling over time |
| Release engineer | Uses a dashboard to monitor the build progress of a microservice artifact (e.g., a container image) to, for example, prevent performance regressions |
| Product manager | Uses a CD dashboard to monitor deployment progress of microservices |
| Business stakeholder | Uses a dashboard to track weekly revenue and SLA violations |

Let’s now have a look at a concrete example of a cloud-native system, the signal sources, and telemetry.

## 1.4 Example microservices app

We will be using Weaveworks’s microservices application, Sock Shop (`https://github.com/microservices-demo/microservices-demo`), to explore observability. Figure 1.2 shows its overall architecture. The Sock Shop example app is a simple demo app built using different languages and frameworks (Spring Boot, Go kit, and Node.js) and is packaged using Open Container Initiative–compliant container images. This makes it widely usable from container orchestrators, such as Kubernetes, Docker Compose, and ECS, as well as in FaaSs, such as AWS Lambda. We will show the example app running in Kubernetes.

![](OEBPS/Images/01-02.png)

Figure 1.2 The example microservices app we use for observability exploration

Tip Throughout the book, you will find me using a didactical device to explain concepts along the boundaries of logs, metrics, and traces. This is purely to structure the content and make it easier for you to learn and find information. The practical reality is much messier, since projects and products have the tendency to expand their scopes; therefore, a clear-cut explanation, such as “This thing is for logs” or “Use XYZ for metrics,” is in most cases not applicable in the real world.

To set up the example microservices app, you need a Kubernetes cluster. Then, follow the steps in the following listing.

Listing 1.1 Local observability

```
$ git clone https:/ /github.com/microservices-demo/microservices-demo.git &&
  \  cd microservices-demo/deploy/kubernetes/                ❶
 
$ kubectl create namespace sock-shop                         ❷
 
$ kubectl apply -f complete-demo.yaml                        ❸
 
$ kubectl -n sock-shop port-forward svc/front-end 8888:80    ❹
```

❶ Clones the example app repo and changes into the directory where the Kubernetes deployment YAML docs are located
❷ Creates a Kubernetes namespace that acts as a logical home for our app
❸ Creates all necessary Kubernetes resources (deployments, services, etc.) that together make up the app; if you want to keep an eye on it, waiting until all resources are ready, you can (in a different terminal session) use the `watch kubectl -n sock-shop get deploy,pod,svc` command.
❹ Once all pods are ready, you can make the frontend accessible on your local machine using the `port-forward` command.

If you successfully set up the Sock Shop app, you can access the entry point (the `front-end` service) in your browser. Head over to `http://localhost:8888`, and you should see something akin to what is depicted in figure 1.3.

![](OEBPS/Images/01-03.png)

Figure 1.3 Screenshot of the Sock Shop example microservices app frontend

Next, you want to generate some traffic, so log in (in the right-hand upper corner) with the username `user` and the password `password`, and then add some items to your shopping cart. Now, what about observability? Let’s have a look at a concrete example end to end.

The Sock Shop example app emits various signals. Let’s take a closer look at two signal types, logs and metrics, to explore what exactly is emitted and what would be necessary to introduce observability for a given role.

In terms of the logs signal type, Sock Shop services emit a variety of logs. One example, looking at the `orders` service, is as follows (output edited to fit):

```
$ kubectl logs deploy/orders
2021-10-25 14:33:10.649  INFO [bootstrap,,,] 7 --- [           main] s.c.a.
AnnotationConfigApplicationContext : Refreshing org.springframework.context.
annotation.AnnotationConfigApplicationContext@51521cc1: startup date
[Mon Oct 25 14:33:10 GMT 2021]; root of context hierarchy
2021-10-25 14:33:16.838  INFO [bootstrap,,,] 7 --- [           main] tratio
Delegate$BeanPostProcessorChecker : Bean 'configurationPropertiesRebinderAu
toConfiguration' of type [class org.springframework.cloud.autoconfigure.Con
figurationPropertiesRebinderAutoConfiguration$$EnhancerBySpringCGLIB$$b894f
39] is not eligible for getting processed by all BeanPostProcessors (for ex
ample: not eligible for auto-proxying)
...
```

In this case, the source is the `orders` service, and the signal type is logs.

Note Nearly everyone uses logs (in contrast to metrics or even traces), yet there is much less standardization and interoperability going on in this signal type compared to others. While OpenTelemetry is, at the time of publication, stable for traces and almost stable for metrics, we’re still working in the community to achieve a similar level of coverage for logs.

To make the logs actionable, for a developer to fix a bug, for example, we would need the following:
* An agent (like `Fluent Bit`) to route the logs to a destination
* A destination, such as `OpenSearch` or `CloudWatch`

The services also expose metrics in the Prometheus exposition format (e.g., looking at the `front-end` service we forwarded to the local machine earlier). The following output is edited to fit:

```
$ curl localhost:8888/metrics
# HELP process_cpu_user_seconds_total Total user CPU time spent in seconds.
# TYPE process_cpu_user_seconds_total counter
process_cpu_user_seconds_total 156.37216999999978
...
```

In this case, the source is the `front-end` service, and the signal type is metrics.

Note

When talking with customers, I notice that many are facing the same challenge: feeling the need to collect and retain all metrics because “we might need them.” Try to prune as much as possible on ingest and check if metrics that land in destinations are actually used. These are some questions you can ask yourself about whether collecting a particular metric is necessary:
* Is that metric used in an alert? Is it used in a dashboard?
* Can you think of a use case where you could derive insights from the metric? Would an aggregate be sufficient?
* Would a trace be a better choice here?

To make the metrics actionable, for an SRE to assess indicators, for example, we would need the following:
* An agent (e.g., `Prometheus` or `OpenTelemetry collector`) to scrape the metrics
* A destination, such as `Prometheus` or, for long-term storage and federation, `Cortex` or `Thanos`

For now, we will not consider traces, since they require a little bit more setup (both from the telemetry side and the destination), but in a nutshell, you can think of traces as providing an overview of the execution across services, including the duration and status of the service that successfully handled the request.

We’ve had an initial exposure to logs and metrics now. Before we go deeper into cloud observability throughout the rest of this book, let’s finish this chapter by looking at the challenges you will need to keep in mind when dealing with cloud-native systems and how observability can help to address them.

## 1.5 Challenges and how observability helps

Cloud-native environments and the apps running on them face several challenges. In this subsection, we discuss the most pressing of these and show how observability can help.
