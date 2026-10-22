
integrated with pessimistic locking. This ensures the producer’s side that no other consumer will be able to process the message.

## Event-carried state transfer

Event-carried state transfer (ECST) messages notify subscribers about changes in the producer’s internal state. Contrary to event notification messages, ECST messages include all the data reflecting the change in the state.

ECST messages can come in two forms. The first is a complete snapshot of the modified entity’s state:

```
{
  "type": "customer-updated",
  "event-id": "6b7ce6c6-8587-4e4f-924a-cec028000ce6",
  "customer-id": "01b18d56-b79a-4873-ac99-3d9f767dbe61",
  "timestamp": 1615728520,
  "payload": {
    "first-name": "Carolyn",
    "last-name": "Hayes",
    "phone": "555-1022",
    "status": "follow-up-set",
    "follow-up-date": "2021/05/08",
    "birthday": "1982/04/05",
    "version": 7
  }
}
```

The ECST message in the preceding example includes a complete snapshot of a customer’s updated state. When operating large data structures, it may be reasonable to include in the ECST message only the fields that were actually modified:

```
{
  "type": "customer-updated",
  "event-id": "6b7ce6c6-8587-4e4f-924a-cec028000ce6",
  "customer-id": "01b18d56-b79a-4873-ac99-3d9f767dbe61",
  "timestamp": 1615728520,
  "payload": {
    "status": "follow-up-set",
    "follow-up-date": "2021/05/10",
    "version": 8
    }
}
```

Whether ECST messages include complete snapshots or only the updated fields, a stream of such events allows consumers to hold a local cache of the entities’ states and work with it. Conceptually, using event-carried state transfer messages is an asynchronous data replication mechanism. This approach makes the system more fault tolerant, meaning that the consumers can continue functioning even if the producer is not available. It is also a way to improve the performance of components that have to process data from multiple sources. Instead of querying the data sources each time the data is needed, all the data can be cached locally, as shown in Figure 15-4.

![](img/b742a71dea5d5dd509847f82600a6ad1_342_0.png)

Figure 15-4. Backend for frontend

## Domain event

The third type of event message is the domain event that we described in Chapter 6. In a way, domain events are somewhere between event notification and ECST messages: they both describe

a significant event in the business domain, and they contain all the data describing the event. Despite the similarities, these types of messages are conceptually different.

## Domain events versus event notification

Both domain events and event notifications describe changes in the producer’s business domain. That said, there are two conceptual differences.

First, domain events include all the information describing the event. The consumer does not need to take any further action to get the complete picture.

Second, the modeling intent is different. Event notifications are designed with the intent to alleviate integration with other components. Domain events, on the other hand, are intended to model and describe the business domain. Domain events can be useful even if no external consumer is interested in them. That’s especially true in event-sourced systems, where domain events are used to model all possible state transitions. Having external consumers interested in all the available domain events would result in suboptimal design. We will discuss this in greater detail later in this chapter.

## Domain events versus event-carried state transfer

The data contained in domain events is conceptually different from the schema of a typical ECST message.

An ECST message provides sufficient information to hold a local cache of the producer’s data. No single domain event is supposed to expose such a rich model. Even the data included in a specific domain event is not sufficient for caching the aggregate’s state, as other domain events that the consumer is not subscribed to may affect the same fields.

Furthermore, as in the case of notification events, the modeling intent is different for the two types of messages. The data included in domain events is not intended to describe the aggregate’s state. Instead, it describes a business event that happened during its lifecycle.

## Event types: Example

Here is an example that demonstrates the differences between the three types of events. Consider the following three ways to represent the event of marriage:

```
eventNotification = {
  "type": "marriage-recorded",
  "person-id": "01b9a761",
  "payload": {
    "person-id": "126a7b61",
    "details": "/01b9a761/marriage-data"
  }
};

ecst = {
  "type": "personal-details-changed",
  "person-id": "01b9a761",
  "payload": {
    "new-last-name": "Williams"
  }
};

domainEvent = {
  "type": "married",
  "person-id": "01b9a761",
  "payload": {
    "person-id": "126a7b61",
    "assumed-partner-last-name": true
  }
};
```

marriage-recorded is an event notification message. It contains no information except the fact that the person with the specified ID got married. It contains minimal information about the event, and the

consumers interested in more details will have to follow the link in the details field.

personal-details-changed is an event-carried state transfer message. It describes the changes in the person’s personal details, namely that their last name has been changed. The message doesn’t explain the reason why it has changed. Did the person get married or divorced?

Finally, married is a domain event. It is modeled as close as possible to the nature of the event in the business domain. It includes the person’s ID and a flag indicating whether the person assumed their partner’s name.

## Designing Event-Driven Integration

As we discussed in Chapter 3, software design is predominantly about boundaries. Boundaries define what belongs inside, what remains outside, and most importantly, what goes across the boundaries—essentially, how the components are integrated with one another. The events in an EDA-based system are first-class design elements, affecting both how the components are integrated and the components’ boundaries themselves. Choosing the correct type of event message is what makes (decouples) or breaks (couples) a distributed system.

In this section, you will learn heuristics for applying different event types. But first, let’s see how to use events to design a strongly coupled, distributed big ball of mud.

## Distributed Big Ball of Mud

Consider the system shown in Figure 15-5.

The CRM bounded context is implemented as an event-sourced domain model. When the CRM system had to be integrated with the

Marketing bounded context, the teams decided to leverage the event-sourced data model’s flexibility and let the consumer—in this case, Marketing—subscribe to the CRM’s domain events and use them to project the model that fits their needs.

When the AdsOptimization bounded context was introduced, it also had to process the information produced by the CRM bounded context. Again, the teams decided to let AdsOptimization subscribe to all domain events produced in the CRM and project the model that fits AdsOptimization’s needs.

![](img/b742a71dea5d5dd509847f82600a6ad1_346_0.png)

Figure 15-5. Strongly coupled distributed system

Interestingly, both the Marketing and AdsOptimization bounded contexts had to present the customers’ information in the same format, and hence ended up projecting the same model out of the

CRM’s domain events: a flattened snapshot of each customer’s state.

The Reporting bounded context subscribed only to a subset of domain events published by the CRM and used as event notification messages to fetch the calculations performed in the AdsOptimization context. However, since both AdsOptimization bounded contexts use the same events to trigger their calculations, to ensure that the Reporting model is updated the AdsOptimization context introduced a delay. It processed messages five minutes after receiving them.

This design is terrible. Let’s analyze the types of coupling in this system.

## Temporal Coupling

The AdsOptimization and Reporting bounded contexts are temporally coupled: they depend on a strict order of execution. The AdsOptimization component has to finish its processing before the Reporting module is triggered. If the order is reversed, inconsistent data will be produced in the Reporting system.

To enforce the required execution order, the engineers introduced the processing delay in the Reporting system. This delay of five minutes lets the AdsOptimization component finish the required calculations. Obviously, this doesn’t prevent incorrect order of execution:

- AdsOptimization may be overloaded and unable to finish the processing in five minutes.
- A network issue may delay the delivery of incoming messages to the AdsOptimization service.
- The AdsOptimization component can experience an outage and stop processing incoming messages.
