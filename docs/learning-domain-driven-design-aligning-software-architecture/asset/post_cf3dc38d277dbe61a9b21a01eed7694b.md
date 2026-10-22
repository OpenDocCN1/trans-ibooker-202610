
Furthermore, no system operation can assume a multi-aggregate transaction. A change to an aggregate’s state can only be committed individually, one aggregate per database transaction.

The one aggregate instance per transaction forces us to carefully design an aggregate’s boundaries, ensuring that the design addresses the business domain’s invariants and rules. The need to commit changes in multiple aggregates signals a wrong transaction boundary, and hence, wrong aggregate boundaries.

This seems to impose a modeling limitation. What if we need to modify multiple objects in the same transaction? Let’s see how the pattern addresses such situations.

## Hierarchy of entities

As we discussed earlier in the chapter, we don’t use entities as an independent pattern, only as part of an aggregate. Let’s see the fundamental difference between entities and aggregates, and why entities are a building block of an aggregate rather than of the overarching domain model.

There are business scenarios in which multiple objects should share a transactional boundary; for example, when both can be modified simultaneously or the business rules of one object depend on the state of another object.

DDD prescribes that a system’s design should be driven by its business domain. Aggregates are no exception. To support changes to multiple objects that have to be applied in one atomic transaction, the aggregate pattern resembles a hierarchy of entities, all sharing transactional consistency, as shown in Figure 6-3.

![](img/b742a71dea5d5dd509847f82600a6ad1_138_0.png)

Figure 6-3. Aggregate as a hierarchy of entities

The hierarchy contains both entities and value objects, and all of them belong to the same aggregate if they are bound by the domain’s business logic.

That’s why the pattern is named “aggregate”: it aggregates business entities and value objects that belong to the same transaction boundary.

The following code sample demonstrates a business rule that spans multiple entities belonging to the aggregate’s boundary—“If an agent didn’t open an escalated ticket within 50% of the response time limit, it is automatically reassigned to a different agent”:

```
01 public class Ticket
02 {
03     ...
04     List<Message> _messages;
05     ...
06
07     public void Execute(EvaluateAutomaticActions cmd)
08     {
09         if (this.IsEscalated && this.RemainingTimePercentage < 0.5
10         &&
11             GetUnreadMessagesCount(for: AssignedAgent) > 0)
```
```
11        {
12            _agent = AssignNewAgent();
13        }
14    }
15
16    public int GetUnreadMessagesCount(UserId id)
17    {
18        return _messages.Where(x => x.To == id &&
!x.WasRead).Count();
19    }
20
21    ...
22 }
```

The method checks the ticket’s values to see whether it is escalated and whether the remaining processing time is less than the defined threshold of 50% (line 9). Furthermore, it checks for messages that were not yet read by the current agent (line 10). If all conditions are met, the ticket is requested to be reassigned to a different agent.

The aggregate ensures that all the conditions are checked against strongly consistent data, and it won’t change after the checks are completed by ensuring that all changes to the aggregate’s data are performed as one atomic transaction.

## Referencing other aggregates

Since all objects contained by an aggregate share the same transactional boundary, performance and scalability issues may arise if an aggregate grows too large.

The consistency of the data can be a convenient guiding principle for designing an aggregate’s boundaries. Only the information that is required by the aggregate’s business logic to be strongly consistent should be a part of the aggregate. All information that can be eventually consistent should reside outside of the aggregate’s boundary; for example, as a part of another aggregate, as shown in Figure 6-4.

![](img/b742a71dea5d5dd509847f82600a6ad1_140_0.png)

Figure 6-4. Aggregate as consistency boundary

The rule of thumb is to keep the aggregates as small as possible and include only objects that are required to be in a strongly consistent state by the aggregate’s business logic:

```
public class Ticket
{
    private UserId        _customer;
    private List<ProductId> _products;
    private UserId        _assignedAgent;
    private List<Message> _messages;

    ...
}
```

In the preceding example, the Ticket aggregate references a collection of messages, which belong to the aggregate’s boundary. On the other hand, the customer, the collection of products that are relevant to the ticket, and the assigned agent do not belong to the aggregate and therefore are referenced by its ID.

The reasoning behind referencing external aggregates by ID is to reify that these objects do not belong to the aggregate’s boundary, and to ensure that each aggregate has its own transactional boundary.

To decide whether an entity belongs to an aggregate or not, examine whether the aggregate contains business logic that can lead to an invalid system state if it will work on eventually consistent data. Let’s go back to the previous example of reassigning the ticket if the current agent didn’t read the new messages within 50% of the response time limit. What if the information about read/unread messages would be eventually consistent? In other words, it would be reasonable to receive reading acknowledgment after a certain delay. In that case, it’s safe to expect a considerable number of tickets to be unnecessarily reassigned. That, of course, would corrupt the system’s state. Therefore, the data in the messages belongs to the aggregate’s boundary.

## The aggregate root

We saw earlier that an aggregate’s state can only be modified by executing one of its commands. Since an aggregate represents a hierarchy of entities, only one of them should be designated as the aggregate’s public interface—the aggregate root, as shown in Figure 6-5.

![](img/b742a71dea5d5dd509847f82600a6ad1_142_0.png)

Figure 6-5. Aggregate root

Consider the following excerpt of the Ticket aggregate:

```
public class Ticket
{
    ...
    List<Message> _messages;
    ...

    public void Execute(AcknowledgeMessage cmd)
    {
        var message = _messages.Where(x => x.Id == cmd.id).First();
        message.WasRead = true;
    }
    ...
}
```

In this example, the aggregate exposes a command that allows marking a specific message as read. Although the operation modifies an instance of the Message entity, it is accessible only through its aggregate root: Ticket.

In addition to the aggregate root’s public interface, there is another mechanism through which the outer world can communicate with aggregates: domain events.

## Domain events

A domain event is a message describing a significant event that has occurred in the business domain. For example:

- Ticket assigned
- Ticket escalated
- Message received

Since domain events describe something that has already happened, their names should be formulated in the past tense.

The goal of a domain event is to describe what has happened in the business domain and provide all the necessary data related to the event. For example, the following domain event communicates that the specific ticket was escalated, at what time, and for what reason:

```
{
  "ticket-id": "c9d286ff-3bca-4f57-94d4-4d4e490867d1",
  "event-id": 146,
  "event-type": "ticket-escalated",
  "escalation-reason": "missed-sla",
  "escalation-time": 1628970815
}
```

As with almost everything in software engineering, naming is important. Make sure the names of the domain events succinctly reflect exactly what has happened in the business domain.

Domain events are part of an aggregate’s public interface. An aggregate publishes its domain events. Other processes, aggregates, or even external systems can subscribe to and execute their own logic in response to the domain events, as shown in Figure 6-6.
