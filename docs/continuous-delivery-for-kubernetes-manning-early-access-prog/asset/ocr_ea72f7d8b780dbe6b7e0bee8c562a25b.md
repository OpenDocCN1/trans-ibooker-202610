Driven approach, part of these interactions will be hidden away from the Kubernetes land. In other words, by using `kubectl` or a Kubernetes Dashboard, we wouldn’t understand who is producing events, where the events are sent to be routed, or which component is consuming what kind of events.

Luckily for us, that is precisely the problem that Knative Eventing is solving. Chapter 5 looked at how Knative Serving built on top of native Kubernetes resources to simplify the implementation of different release strategies and provided advanced traffic routing. Knative Serving provided higher-level abstractions to manage Services and how they can be automatically scaled up and down based on demand.

In the following section, we will take a look at Knative Eventing which provides abstractions for Event Producers (Sources), Event Consumers (Sinks), Event Routers (Brokers) and Event Filters (Triggers).

# 6.2 Building Event-Driven Applications with Knative Eventing

Knative Eventing is independent of Knative Serving. You can install Knative Eventing even if you don’t have Knative Serving installed in your cluster, but of course, your Event-Driven applications can benefit from autoscaling and advanced traffic management. As we discussed previously, Producers and Consumers can be regular Knative or Kubernetes Services, and they usually are.

Knative Eventing, as Knative Serving, builds on top of the Kubernetes API to provide abstractions for Event Consumers (sources), Event Producers (sinks), Event Routers (brokers) and Event Filters (triggers). While the natural starting points would be Consumers and Producers, we will start with Event Routers and Filters.

![](bbox=[0.17, 0.7, 0.62, 0.9])

Sources (Producers) produce events and send them to a Broker (Router). The Broker will look at the defined Triggers (filters) and only send to the Sinks (Consumers) the events that match with the filter specified inside the Trigger.

Figure 6.6 Knative Eventing Sources, Brokers, Triggers and Sinks