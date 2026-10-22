CRM’s domain events: a flattened snapshot of each customer’s state.

The Reporting bounded context subscribed only to a subset of domain events published by the CRM and used as event notification messages to fetch the calculations performed in the AdsOptimization context. However, since both AdsOptimization bounded contexts use the same events to trigger their calculations, to ensure that the Reporting model is updated the AdsOptimization context introduced a delay. It processed messages five minutes after receiving them.

This design is terrible. Let’s analyze the types of coupling in this system.

# Temporal Coupling

The AdsOptimization and Reporting bounded contexts are temporally coupled: they depend on a strict order of execution. The AdsOptimization component has to finish its processing before the Reporting module is triggered. If the order is reversed, inconsistent data will be produced in the Reporting system.

To enforce the required execution order, the engineers introduced the processing delay in the Reporting system. This delay of five minutes lets the AdsOptimization component finish the required calculations. Obviously, this doesn’t prevent incorrect order of execution:

+   - AdsOptimization may be overloaded and unable to finish the processing in five minutes.
+   - A network issue may delay the delivery of incoming messages to the AdsOptimization service.
+   - The AdsOptimization component can experience an outage and stop processing incoming messages.