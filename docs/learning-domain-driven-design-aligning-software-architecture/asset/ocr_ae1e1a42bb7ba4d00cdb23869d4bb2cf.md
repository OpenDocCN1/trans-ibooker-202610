```
"follow-up-date": "2021/05/10",
"version": 8
    }
}
```

Whether ECST messages include complete snapshots or only the updated fields, a stream of such events allows consumers to hold a local cache of the entities’ states and work with it. Conceptually, using event-carried state transfer messages is an asynchronous data replication mechanism. This approach makes the system more fault tolerant, meaning that the consumers can continue functioning even if the producer is not available. It is also a way to improve the performance of components that have to process data from multiple sources. Instead of querying the data sources each time the data is needed, all the data can be cached locally, as shown in Figure 15-4.

![](bbox=[0.13, 0.46, 0.87, 0.75])

Figure 15-4. Backend for frontend

# Domain event

The third type of event message is the domain event that we described in Chapter 6. In a way, domain events are somewhere between event notification and ECST messages: they both describe