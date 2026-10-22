In conjunction with the delivery types, the broker maintains delivery guarantees from producer to consumer per message type in an agreement called a *contract*. The typical delivery types are shown in Figure 8-2: at-most-once, at-least-once, and exactly once. The diagram shows the important relationship between when the producer sends a message and the expectation of how the consumer receives the message.

![](bbox=[0.12, 0.26, 0.88, 0.56])

Figure 7-2. Delivery Guarantees

# At-most-once

The lowest guarantee allows data created by the producer to be skipped by the consumer. For example, this might be used when a consumer only needs to react to the most current information. If a consumer is taken offline for any period, the correct action once back online is to pick up processing on the latest data and ignore anything previously delivered. The critical downside to understand is that data loss is possible by design.

# At-least-once

This guarantee is the opposite side of at-most-once. Data created by the producer is guaranteed to be picked up by a consumer. The added aspect