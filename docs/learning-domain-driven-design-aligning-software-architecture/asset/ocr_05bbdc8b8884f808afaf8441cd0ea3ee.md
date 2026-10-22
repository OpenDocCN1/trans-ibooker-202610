snapshot. This pattern, shown in Figure 7-2, implements the following steps:

+   - A process continuously iterates new events in the event store, generates corresponding projections, and stores them in a cache.
+   - An in-memory projection is needed to execute an action on the aggregate. In this case:
+     - The process fetches the current state projection from the cache.
+     - The process fetches the events that came after the snapshot version from the event store.
+     - The additional events are applied in-memory to the snapshot.