# Event Streams for Data Mesh

An event is published to an event stream for use by any interested subscribers. But what are the properties of an event stream and are they any different than a queue? Or a messaging system? What makes up an event, and how do we ensure we correctly compose reusable event streams? In this chapter, we’ll take a good look at events and event streams, including requirements, differentiation from other similar architectures, and some of the modes of use that they unlock for our consumer services.

First, let's start with the event. An *event* is a well-defined record of an occurrence containing all of the information about what happened. Events are commonly based on business entities or on relationships between entities. For a simple automotive example, we could expect to see entity events detailing information about *items*, *orders*, and *coupons*. In terms of relationships between entities, you may expect to see events such as *item_added_to_cart* and *coupon_applied_to_order*.

We’ll get more into precisely what events are, how they’re structured, and the supporting technologies we’ll need to use in a moment. First, it’s important to understand a few key concepts about the role that event streams play in an event-driven data mesh.

Figure 3-1 shows an extension of the automotive domain bounded context from Figure 2-1. The internal implementation uses a relational database, with tables representing the components/entities required to compose an automobile. The anti-corruption layer provides the REST API for operational use cases, such as requesting the assembly of a new automobile and modifications to existing inventory.

Additionally, the domain publishes two data products to event streams—one based on items and the other based on orders. Specialized business logic extracts and isolates the internal model from the data published to the event stream. Whatever you