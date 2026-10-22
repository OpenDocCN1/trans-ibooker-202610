Let’s see how the events can be adjusted to improve the design dramatically.

Exposing all the domain events constituting the CRM’s data model couples the subscribers to the producer’s implementation details. The implementation coupling can be addressed by exposing either a much more restrained set of events or a different type of events.

The Marketing and AdsOptimization subscribers are functionally coupled to each other by implementing the same business functionality.

Both implementation and functional coupling can be tackled by encapsulating the projection logic in the producer: the CRM bounded contexts. Instead of exposing its implementation details, the CRM can follow the consumer-driven contract pattern: project the model needed by the consumers and make it a part of the bounded context’s published language—an integration-specific model, decoupled from the internal implementation model. As a result, the consumers get all the data they need and are not aware of the CRM’s implementation model.

To tackle the temporal coupling between the AdsOptimization and Reporting bounded contexts, the AdsOptimization component can publish an event notification message, triggering the Reporting component to fetch the data it needs. This refactored system is shown in Figure 15-6.