
- Developing the design and structure of your APIs is an important architectural activity. Align your API design with data product design when defining APIs for collaboration with other domains. Remember that the underlying application database for APIs and data products is always the same!
- Separate guidance for APIs meant for internal-application development from APIs meant for collaboration with other domains. Internal APIs, for example, can be technical, while APIs offered to other domains should be stable, self-explanatory, and easy to consume.
- Set standards for platforms and services and align these with the scope and roles the APIs play. For example, APIs facing external service consumers must be published on a different API gateway that better matches the increased security requirements.
- Publish guidelines and best practices for API design. Make your API community the owner of this content.
- Pay attention to microservices architectures. Consider adding boundaries rather than leaving teams to face the complexity of hundreds of services.
- Pay great attention to composite APIs because they can make your architecture complex and tightly coupled.

Because of their REST design, using APIs is also a great way to go distributed with all of your data. You can deploy multiple API gateways and service meshes at the same time, potentially with each team or domain using its own API gateway for providing its services. You can stay in control by using metadata repositories that hold information about which services are available and how they're consumed.

The modularity and request/response model of the API architecture also allow highly modular microsolutions for specific use cases. This makes the architecture highly scalable. The caveat here is that you must follow the same design principles discussed in Chapter 2 for the domain architecture. APIs should be seen as products used to generate, send, and receive data in real time. They must be owned and optimized for readability and easy consumption. They should use the domain's ubiquitous language and shouldn't incorporate business logic from other domains.

In the next chapter, we'll focus on asynchronous messaging and event-driven communication and see what lessons we can learn from that. See you there!

## Event and Notification Management

Event-driven data architectures are the new paradigm. With these, organizations can transform their businesses into real-time, data-driven experiences. But while they're powerful, they're also overly complex. They require many services, additional components, and frameworks. They are also not a solution for every type of problem. It's worth discussing these considerations when implementing any type of event-driven communication.

In this chapter, we'll concentrate on event and notification management. These are complex areas because they overlap with API management and data product architecture design. We'll look at things like asynchronous communication, event-driven architectures, modern cloud technologies, consistency models, event types, and more. By the end of this chapter, you'll have a good understanding of what event-driven architectures can bring to your organization, and of the trade-offs involved.

There are a few things I'd like to mention up front. First, not all data practitioners are familiar with event-driven architectures. I'll explain some basics and concentrate on event distribution in the context of data management. Second, event-driven architectures can become complex; a complete discussion of this topic could easily fill a whole book. I'll focus on the key aspects and use a reference example to demonstrate the governance model and the most important considerations to keep in mind.

1. Designing Event-Driven Systems by Ben Stopford (O'Reilly) is a great resource if you want to learn more about event-driven architectures.

## Introduction to Events

Before we discuss architecture and governance, I want to examine event data, what it looks like, and why asynchronous communication makes a difference. Event data is information about a change that occurs at a point in time (an _event_). Event data provides a competitive advantage: faster results make insights more relevant, and greater responsiveness leads to higher customer satisfaction. Instead of waiting minutes or hours, you can react immediately. Fraud can be detected the moment it happens. You can see where your web visitors are coming from, interact with them, and improve their customer experience as they navigate your website. Data delivered in a stream of events can be of enormous value to organizations, and its sources can vary hugely, from Internet of Things (IoT) devices to smartphone clicks, in-game player activity, social networks, and changes to traditional operational systems.

Unlike data delivered in offline batches or in response to API requests (in real time), event data focuses on interacting with and reacting to events. It’s about _asynchronously_ connecting applications and systems. Event data emphasizes high throughput and can be either stateless or stateful. It allows you to exchange information about changes that occur at specific points in time and to move the application state between locations, which enables distributed, up-to-date, materialized views.

Event-driven architectures often use event streaming platforms, which process the inbound events in flight, as the data is streaming through the platform. These software platforms have different "faces" and support various capabilities. For example, they allow you to:

- Manipulate the stream of events by performing real-time ETL and data integration processes
- Run real-time analytics by performing an analysis of a window of events to detect patterns or correlations
- Store data and move it between storage locations in real time
- Trigger functions and issue updates to other systems

There’s some overlap with the way data products are stored and APIs are used. Because of that, these event-driven patterns require very good data management. For example, one event may trigger a slew of messages or calls to many different services. From a data management standpoint, you want to be able to track the replies to all of those.

## Notifications Versus Carried State

Events come in different shapes, some of which have more overlap with data distribution and management. One method of distributing events is called event-carried state transfer. With this approach, the event itself carries the full state related to the change, so the consumer has all the information it needs and doesn’t have to make a separate request for the state after being notified of the event. The consumer can even use these events to build up its own local copy of the data. Example 6-1 shows what this might look like for an "add customer" event. An event can also be shaped in such a way that it contains both old and new data, so, for example, for an "update customer" event, consumers can see exactly what information has been changed.

### Example 6-1. Example of an event that carries the full state relating to a customer change

```
{
  "id": "321125215",
  "firstName": "Adolfo",
  "lastName": "Prosacco",
  "gender": "male",
  "birthday": "1997-05-01",
  "address": {
    "streetAddress": "17795 Louisiana 182",
    "city": "Baldwin",
    "state": "Louisiana",
    "postalCode": "70514",
    "country": "United States"
  },
  "phoneNumber": [
    {
      "type": "home",
      "number": "(813) 872-1350"
    },
    {
      "type": "mobile",
      "number": "(202) 555-0154"
    }
  ]
}
```

An alternative approach is to just send a notification about the event, without the full state. In this case, the event will only contain the bare minimum information: a reference to the state that was changed. Although Example 6-2 has been oversimplified, it’s obvious that a consumer of such an event will need to make a call back to the producer to retrieve more information about the change.
