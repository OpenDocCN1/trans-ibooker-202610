# Domain event

A message describing an event in the producer’s business domain.

Using inappropriate types of events will derail an EDA-based system, inadvertently turning it into a big ball of mud. To choose the correct type of events for integration, evaluate the bounded contexts’ consistency requirements and be wary of exposing implementation details. Design an explicit set of public and private events. Finally, ensure that the system delivers the messages, even in the face of technical issues and outages.

# Exercises

+   1. Which of the following statements is/are correct?
+   	a. Event-driven architecture defines the events intended to travel across components’ boundaries.
+   	b. Event sourcing defines the events that are intended to stay within the bounded context’s boundary.
+   	c. Event-driven architecture and event sourcing are different terms for the same pattern.
+   	d. A and B are correct.
+   2. What type of event is best suited for communicating changes in state?
+   	a. Event notification.
+   	b. Event-carried state transfer.
+   	c. Domain event.