can generate an audit log of the changes that occurred, this log is used primarily for auditing purposes and not for driving business logic.

There are some limitations to the CRUD model that may make event sourcing an attractive alternative. For one, operations must be processed directly against the data store as they are invoked. Under heavy use, this can significantly slow down operations and result in timeouts and failures. Second, high concurrency operations on the same entities can result in data conflicts and failed transactions, further increasing load on the system.

But the CRUD model also contains several distinct advantages. Though it depends largely on the database, most CRUD implementations offer strong read-after-write consistency. It’s also fairly intuitive and simple to use, with lots of tools and frameworks supporting it. For many software developers, this is the first model of maintaining state that they encounter. Figure 7-2 shows a series of CRUD events (one create, two updates) applying changes to the refrigerator state. The state is completely mutable, and only the updated state is retained after a create or update command is applied.

![](bbox=[0.145, 0.415, 0.855, 0.72])

Figure 7-2. Using CRUD commands to update the contents of the refrigerator, reflected in the database

Under the event sourcing architecture, these create, update, and destroy operations are instead modeled as events that are written to a durable append-only log that retains them indefinitely. It is not uncommon to use a single database table to act as the append-only log. It is also possible to use an event broker like Apache Kafka to host the append-only log, although this does introduce additional latency. In either