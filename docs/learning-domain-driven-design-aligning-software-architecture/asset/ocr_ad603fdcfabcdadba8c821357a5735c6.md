The relay can continuously query the database for unpublished events. Proper indexes have to be in place to minimize the load on the database induced by the constant polling.

# Push: transaction log tailing

Here we can leverage the database’s feature set to proactively call the publishing relay when new events are appended. For example, some relational databases enable getting notifications about updated/inserted records by tailing the database’s transaction log. Some NoSQL databases expose committed changes as streams of events (e.g., AWS DynamoDB Streams).

It’s important to note that the outbox pattern guarantees delivery of the messages at least once: if the relay fails right after publishing a message but before marking it as published in the database, the same message will be published again in the next iteration.

Next, we’ll take a look at how we can leverage the reliable publishing of domain events to overcome some of the limitations imposed by aggregate design principles.

# Saga

One of the core aggregate design principles is to limit each transaction to a single instance of an aggregate. This ensures that an aggregate’s boundaries are carefully considered and encapsulate a coherent set of business functionality. But there are cases when you have to implement a business process that spans multiple aggregates.

Consider the following example: when an advertising campaign is activated, it should automatically submit the campaign’s advertising