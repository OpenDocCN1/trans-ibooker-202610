# Eventual consistency

This model guarantees that all data will eventually be reflected in the platform, though it may take some time to propagate. Eventual consistency is a weak guarantee and can work for data that isn’t critical.

# Strong consistency

This model guarantees that all events are processed in order. The platform con-firms propagation of any update to all copies of the data; once any entity or node makes an update, all replicas are locked until they have been updated.

Providing strong consistency can be difficult. If the velocity of events is so high that the streaming platform can barely keep up, it can be challenging, if not impossible, to design the system in such a way that all messages will be received correctly. The design constraints imposed by strong consistency are therefore unacceptable for various use cases, making eventual consistency the only possible approach. In these cases, you may need to take additional measures to ensure it is correct. For example, you could use an extra database to persist all the data and reconcile occasionally to ensure all data has been correctly synchronized. You can also use additional tooling, such as CDC, to ensure all changes are correctly delivered, or you could build an extra service to ensure consistency by reading, validating, and redelivering missing messages.

# Processing Methods

There are different processing methods within the eventual and strong consistency models. At-least-once processing ensures that messages don’t get lost, but doesn’t guarantee that all messages are unique and delivered only once. It’s acceptable to have duplicates and for events to be processed multiple times.

Exactly-once processing, also called the delivered-only-once model, guarantees that each event is delivered exactly and only once. No messages are lost, nor are there duplicates. Implementing this model might require additional redelivery patterns. Some engineers use additional identifiers to validate the uniqueness of every message.

The last model is called at-most-once or no-guarantee processing. In this scenario, no guarantees are given that all messages will be delivered correctly. Messages are allowed to get lost or can be delivered multiple times.

You can combine different models, depending on the requirements of your use case and other constraints. An eventual consistency model, for example, can be used to initially deliver messages without acknowledging receipts, and occasionally all messages can be compared, with duplicates removed and missing messages redelivered to ensure that they’ve been delivered exactly and only once.