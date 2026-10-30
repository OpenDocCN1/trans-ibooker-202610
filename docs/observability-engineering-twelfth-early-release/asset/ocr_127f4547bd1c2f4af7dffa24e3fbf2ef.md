volume of emitted events and a cascading failure. Adding a buffer would smooth the data ingestion to the backend. This also improves the reliability of data ingestion into the backend since it is not being saturated by those spikes in volume.

A buffer also acts as an intermediate step to hold data, especially when further processing is desired before sending the data to the backend. Multiple buffers can be used—for example, combining a local disk buffer on the application with a dead-letter queue to protect timeliness of data during volume spikes.

We use Kafka extensively at Slack to achieve this goal of buffering data, and our clusters retain up to three days of events to ensure data consistency after a backend recovers from an outage. In addition to Kafka, we use a limited on-disk buffer in our producer component, allowing our ingestion agents to buffer events if a Kafka cluster becomes unavailable or saturated. End to end, the pipeline is designed to be resilient during service disruptions, minimizing data loss.

# Capacity Management

Often for capacity planning or cost-control reasons, you may want to assign quotas for categories of telemetry and enforce with rate limiting, sampling, or queuing.

# Rate limiting

Since the telemetry data is often produced in relationship to natural user requests, the telemetry data from applications tend to follow unpredictable patterns. A telemetry pipeline can smooth these data spikes for the backend by sending the telemetry to the backend at only a constant rate. If there is more data than this rate, the pipeline can often hold data in memory until the backend can consume it.

If your systems consistently produce data at a higher rate than the backend can consume, your telemetry pipeline can use *rate limits* to mitigate impacts. For instance, you could employ a hard rate limit and drop data that