
## Introduction to Event Types

There are many ways to design events for event-driven architectures. However, some are more suitable than others for use in an event-driven data mesh. This chapter covers the best strategies for designing events for your event-driven data products, including how to avoid the numerous pitfalls that you will encounter along the way.

There are two main types of events that underpin all of event design: the *state* event, as we first introduced in “State Events and Event-Carried State Transfer” on page 55, and the *delta* event, which we’ll cover in more detail in this chapter.

Figure 7-1 shows a simple square wave in steady state, periodically altering from one state to another based on a delta. Similar to this square wave, we model our events to either capture the state itself or the edge that transitions from one state to another.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_164_0.png)

Figure 7-1. State and delta during a change

There are three stages to any occurrence in a system:

- 1. The initial state
- 2. The delta that alters the initial state to produce the final state
- 3. The final state (which is also the initial state for the next change cycle)

The majority of events we encounter can be fully categorized as either state or delta. Looking at events in this way helps separate concerns and focus design efforts:

## State events

State events fully describe the state of an entity at a given point in time and are
the best choice for communicating data products between domains. State events
are typically the most flexible and useful event types for use in an event-driven
data mesh.

## Delta events

These describe the transition between states and typically only contain informa-
tion about what has changed. Delta events have the distinction of being the first
type of event that many event-driven application developers encounter. However,
these events are generally not well-suited for an event-driven data mesh, as we'll
discuss shortly.

While there can be hybrid events that have characteristics of each, these tend to be
less common because they can cause undesirable strong coupling. We'll talk more
about those later in this chapter.

Let's take a look at state events first.

## Expanding on State Events and Event-Carried State Transfer

“State Events and Event-Carried State Transfer” on page 55 introduced both state
events and ECST. As a refresher, a state event showcases the current state of an entity
at a precise moment in time, much like a row in a relational database. ECST allows
for any consumer of the event stream to materialize, aggregate, and store whatever
selection of data it needs in its own domain boundary, to use as it sees fit. In this section, we’ll look at some useful options for extending state events.

State events can contain just the “now” state or they may contain the “before/after”
state (a pattern we’ll cover with “Change-Data Capture” on page 179 in the next chap-
ter). Both options have their own advantages and disadvantages, which we’ll examine
in turn. For starters, let’s take a look at how each of these options affects compaction
of event streams.

There are two main design strategies for defining the structure and contents of ECST events:

## Current state

Contains the full public state at the moment the event was created.

## Before/after state

Contains both the full public state _before_ the event occurred and the full public state _after_ the event occurred.

Let’s look into each of these in detail to get a better understanding of their trade-offs.

## Current State Events

The event contains only the current state of the entity and requires comparison with a previous state event to determine what has changed. For example, an _inventory_ event for a given `item_id` will contain only the latest value for the quantity in stock at that point in time. This design strategy has several main benefits:

### Lean

The state events consume a minimal amount of space in the event stream. Network traffic is also minimized.

### Simple

The event broker stores any previous state events for that entity, such that if you need historical state, you simply rewind and replay your consumer offsets. You can set independent compaction policies for each event stream depending on your consumer’s needs for historical data.

### Compactable

You can keep the number of events in the stream proportional to the key space of the domain.

It also has a few nuances that are not quite drawbacks, but rather things to consider:

### Agnostic to why the state changed

The downstream consumer is not provided with the reason _why_ the data has changed, only the new public state. The reason for this is simple: it removes the ability of consumers to couple on the _internal state transitions_ of the source domain. Think about data in a relational database table—we typically do not communicate _why_ that data has changed in the data itself, and the same holds true for state events (Note: We’ll look at bending this rule a bit with hybrid events a bit later).

### Consumers must maintain state to detect transitions

A consumer must maintain its own state to detect specific changes to certain fields, regardless of how simple or complex its business logic is. For example, a customer changing their address to another country may require you to send them new legal documents, which can differ depending on the country they left and the country they moved to. By making it the consumer’s responsibility to materialize state for tracking transitions, the onus of computing these edges is placed entirely within the domain of the consumer.

Data products built using _current state events_ are flexible and fairly easy to use and should form the basis of most of your data products. If you want to package up state transitions into your event as well, look no further than before/after state.

## Before/After State Events

This strategy relies on providing the state before a transition occurs and the state after it has occurred. Change-data capture (CDC) systems, as covered in “Change-Data Capture” on page 179, regularly make use of the before/after strategy. The following showcases two before/after user events with a simple two-field schema:

```
Key: 26
Value: {
 before: { name: "Adam", country: "Atlantis" },
 after: { name: "Adam", country: "Canada" }
}
```

A follow-up before/after state event that shows the deletion of Key = 26. Note that old data still remains in the before field:

```
Key: 26
Value: {
 before: { name: "Adam", country: "Canada" },
 after: null
}
```

There are some benefits to this design:

### Simple state transitions in a single event

The before/after event showcases every field that has changed within a single transaction, in addition to all of the fields that have not changed. The reason for the change, however, is not included.

### Consumers can detect simple changes without maintaining state

Some consumers can forgo maintaining state if they are only interested in detecting a simple state transition. For example, if we want to send documents to a user who moves from Madagascar to Canada, then our consumer can simply check to see if the before and after fields of the event match their criteria. However, this doesn't work if Adam moves from Madagascar to Ethiopia, and then soon thereafter moves to Canada, causing two events to occur. The consumer business logic would not be able to trigger on this sequence of events since it doesn't maintain any state. In practice, the theoretical stateless consumer is seldomly realized, since the vast majority of services of any reasonable complexity need to maintain state.

There are also a few drawbacks to this design:

### Compaction is difficult

Deleting an event using the before/after logic results in the after field being set to null—but the entire value itself is not null. By default, event brokers like Apache Kafka will not recognize this as a tombstone and thus will not delete it. While it may be technically possible to rewrite the compaction logic, it usually isn't feasible, especially if you are relying heavily on SaaS solutions.

There are, however, some options, depending on which tooling you use. Debezium has worked around this limitation by allowing you to produce a tombstone after the before/after event, generating two events instead of just one.

According to Debezium’s documentation:

> A database DELETE operation causes Debezium to generate two Kafka records:
> 
> * A record that contains "op": "d", the before row data, and some other fields.
> 
> * A tombstone record that has the same key as the deleted row and a value of null. This record is a marker for Apache Kafka. It indicates that log compaction can remove all records that have this key.

### Risk of leftover information

As we saw earlier, previous data may be accidentally maintained indefinitely in the before field unless you issue a series of deletions.

### Doubled data storage and network usage

Before/after events double (on average) the amount of data going over the wire and stored on disk. Consumers, producers, and the event broker each bear part of this load. In some cases this may be trivial. Seldom-updated events or those with low volume are probably nothing to worry about, but extremely high volume event streams can quickly add up the costs. This can be particularly expensive depending on the cross-regional data transfer fees associated with high-availability producer, consumer, and event broker deployments.

I recommend using current state events over before/after when designing event-stream data products. The consumers will need to maintain state for the records they care about for their business processes, but disk space is relatively cheap and they need to select only a subset of the domain that they need. This also simplifies
