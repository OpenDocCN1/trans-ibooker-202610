# 5.1.2  Events: Asynchronous collaboration

Collaboration based on events is asynchronous. That is, the microservice that publishes the events doesn’t call the microservices that subscribe to the events. Rather, the subscribers process new events when they’re ready to process them. When we use HTTP to implement this style of collaboration, the subscribers poll for new events. That polling is what I’ll call *subscribing* to an event feed. Although the polling is made out of synchronous requests, the collaboration is asynchronous because publishing events is independent of any subscriber polling for events.

In figure 5.7, you can see the loyalty program microservice subscribing to events from the special offers microservice. Special offers can publish events whenever something happens in its domain, such as every time a new special offer becomes active. Publishing an event, in this context, means storing the event in the special offers data store. The loyalty program won’t see the event until it makes a call to the event feed on special offers. When that happens is entirely up to the loyalty program. It can happen right after the event is published or at any later point in time.

![](bbox=[0.542, 0.294, 0.902, 0.476])

As with the other types of collaboration, there are two sides to event-based collaboration. One side is the microservice that publishes events through an event feed, and the other is the microservices that subscribe to those events.

# EXPOSING AN EVENT FEED

A microservice can publish events to other microservices via an *event feed*, which is just an HTTP endpoint—at “/events,” for instance—to which that other microservice can make requests and from which it can get event data. Figure 5.8 shows the components

![](bbox=[0.19, 0.647, 0.903, 0.879])