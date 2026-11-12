choose to expose via your anti-corruption layer becomes part of the public API, so be careful about what you add and how you add it.

![](bbox=[0.142, 0.13, 0.858, 0.56])

Figure 3-1. An ecommerce domain including an event-stream API for data products and a REST API for operational concerns

Note that the internal implementation need not be event-driven to extract or use event-driven data products. Chapter 8 covers integrating nonevent-driven systems with event-driven ones and the options you have for bridging the gap. You may also note that we’re incorporating traditional request-response options alongside event streams. This is a very common pattern for systems of record that are not event-driven but need to provide well-formed data products for external consumers. We simply maintain the existing APIs and add on event-driven data products based on consumer needs and the existing data requirements.

Figure 3-2 shows an abstracted implementation of an event-driven automotive domain, complete with domain events specific to the internal operational of the system.

You may choose to expose certain domain events to the outside world, but you must be careful about about which ones you select to expose. We’ll cover event design and