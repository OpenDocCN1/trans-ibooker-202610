# Observability and Debugging Streaming Systems

Streaming AI interfaces are complex and dynamic, making observability critical. You need to see what’s happening across the generation, transport, and render loops in order to diagnose problems.

# Instrument the Streaming Path with Performance Marks

Use the performance API to add marks at key points in the streaming lifecycle:

+   - `mark("prompt-sent")` when the user submits a request
+   - `mark("first-token")` when the first token arrives
+   - `mark("useful-token")` when the first meaningful token appears
+   - `mark("batch-commit")` each time you commit a batch of updates to the DOM
+   - `mark("last-token")` when the stream completes

Measure the intervals between these marks to calculate your streaming metrics. Log them to your analytics or RUM system. Aggregate across users to understand p50, p95, and p99 performance.

Track queue depth: monitor how many items are pending in your rendering pipeline at any given moment. If the queue grows large, it indicates the client is falling behind. Alert on this condition and investigate whether you need to throttle generation, optimize rendering, or both.

Be sure to capture input latency during streams as well. Instrument any user interactions (such as clicks and key presses) that occur while a stream is active. Measure the time from event to visual feedback. High latencies here indicate main-thread contention and warrant optimization.