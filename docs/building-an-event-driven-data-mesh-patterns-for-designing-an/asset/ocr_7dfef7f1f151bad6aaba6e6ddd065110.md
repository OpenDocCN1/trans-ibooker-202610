when the event actually occurred, not when it was recorded to the broker. It’s up to you if you choose to use it, but you’d be better off maintaining the original event timestamps and instead commit to handling out-of-order data at the consumer side.

Out-of-order events are important to consider because they affect the state that a consumer converges to. Two independent consumers that treat out-of-order events differently can arrive at permanently different states and never converge at all. In some situations it may not matter, but in others it may be of critical importance.

![](bbox=[0.173, 0.233, 0.256, 0.33])

I cover the major techniques that stream-processing frameworks use to process events, progress through time, and identify out-of-order data in Building Event-Driven Microservices, Chapter 6. Please refer to this chapter if you are interested in the under-the-hood details.

Since events can be written out of order and aren’t guaranteed to have monotonically increasing timestamps—how do we handle them? Let’s take a look.

# Resolving Late-Arriving Events

Events can only be considered late from the perspective of a consumer, and each consumer is free to maintain its own definition of what is late and what is not. Take for instance a consumer maintaining a time-bound window of aggregated data, say advertisement clicks per hour, derived from the source-aligned Ad Clicks stream. Windowing operations are excellent forcing functions for handling late arriving data, as they require that you answer just one simple question: “Is this specific event part of your time window, or not?”

Figure 10-7 shows the advertisements that the users clicked on, the incrementing offsets, and the event time of when the user clicked on it.

| Value | Time | Offset |
| :--- | :--- | :--- |
| addl:"CoffeeKing-3", userid:"Adam" | 100 | 1 |
| addl:"TeaQueen-7", userid:"Justin" | 500 | 2 |
| addl:"Diet Cola-0", userid:"Renata" | 200 | 3 |
| addl:"CoffeeKing-4", userid:"Adam" | 600 | 4 |
| addl:"KhlavKalash-0", userid:"Homer" | 300 | 5 |
| addl:"TeaQueen-3", userid:"Lisa" | 700 | 6 |
| addl:"MountainDont-0", userid:"Bonko" | 800 | 7 |

Figure 10-7. Out-of-order events representing advertisement clicks, based on event time