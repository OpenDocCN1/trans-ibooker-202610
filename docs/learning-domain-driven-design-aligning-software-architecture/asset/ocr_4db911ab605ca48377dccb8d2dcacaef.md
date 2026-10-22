# Chapter 15. Event-Driven Architecture

As microservices, event-driven architecture (EDA) is ubiquitous in modern distributed systems. Many advise using event-driven communication as the default integration mechanism when designing loosely coupled, scalable, fault-tolerant distributed systems.

Event-driven architecture is often linked to domain-driven design. After all, EDA is based on events, and events are prominent in DDD —we have domain events, and when needed, we even use events as the system’s source of truth. It may be tempting to leverage DDD’s events as the basis for using event-driven architecture. But is this a good idea?

Events are not a kind of secret sauce that you can just pour over a legacy system and turn it into a loosely coupled distributed system. Quite the opposite: careless application of EDA can turn a modular monolith into a distributed big ball of mud.

In this chapter, we will explore the interplay between EDA and DDD. You will learn the essential building blocks of event-driven architecture, common causes for failed EDA projects, and how you can leverage DDD’s tools to design effective, asynchronously integrated systems.

# Event-Driven Architecture

Stated simply, *event-driven architecture* is an architectural style in which a system’s components communicate with one another asynchronously by exchanging event messages (see Figure 15-1).