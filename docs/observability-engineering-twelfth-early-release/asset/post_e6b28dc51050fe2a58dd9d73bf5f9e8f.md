
Because of its understandable and easy-to-implement approach, *constant-probability sampling* is what most people think of when they think of sampling: a constant percentage of data is kept rather than discarded (e.g., keep one out of every ten requests).

When performing analysis of sampled data, you will need to transform the data to reconstruct the original distribution of requests. Suppose that your service is instrumented with both events and metrics, and receives 100,000 requests. It is misleading for your telemetry systems to report receiving only 100 events if each received event represents approximately 1,000 similar events. Other telemetry systems such as metrics will record incremented counters for each of the 100,000 requests that your service has received. Only a fraction of those requests will have been sampled, so your system will need to adjust the aggregation of events to return data that is approximately correct. For a fixed sampling rate system, you can multiply each event by the sampling rate in effect to get the estimated count of total requests and sum of their latency. Scalar distribution properties such as the p99 and median do not need to be adjusted for a constant probability sampling, as they are not distorted by the sampling process.

The basic idea of constant sampling is that, if you have enough volume, any error that comes up will happen again. If that error is happening enough to matter, you’ll see it. However, if you have a moderate volume of data, constant sampling does not maintain the statistical likelihood that you still see what you need to see. Constant sampling is not effective in the following circumstances:

- You care a lot about error cases and not very much about success cases.
- Some customers send orders of magnitude more traffic than others, and you want all customers to have a good experience.
- You want to ensure that a huge increase in traffic on your servers can’t overwhelm your analytics backend.

For an observable system, a more sophisticated approach ensures that enough telemetry data is captured and retained so that you can see into the true state of any given service at any given time.

## Sampling on Recent Traffic Volume

Instead of using a fixed probability, you can dynamically adjust the rate at which your system samples events. Sampling probability can be adjusted upward if less total traffic was received recently, or decreased if a traffic surge is likely to overwhelm the backend of your observability tool. However, this approach adds a new layer of complexity: without a constant sampling rate, you can no longer multiply out each event by a constant factor when reconstructing the distribution of your data.

Instead, your telemetry system will need to use a weighted algorithm that accounts for the sampling probability in effect at the time the event was collected. If one event represents 1,000 similar events, it should not be directly averaged with another event that represents 100 similar events. Thus, for calculating a count, your system must add together the number of *represented* events, not just the number of collected events. For aggregating distribution properties such as the median or p99, your system must expand out each event into many when calculating the total number of events and where the percentile values lie.

For example, suppose you have pairs of values and sample rates: [{1,5}, {3, 2}, {7, 9}]. If you took the median of the values without taking the sample rate into account, you would naively get the value of 3 as a result—because it’s at the center of [1,3,7]. However, you must take the sample rate into account. Using the sample rate to reconstruct the entire set of values can be illustrated by writing out the entire set of values in long form, like so: [1,1,1,1,1,3,3,7,7,7,7,7,7,7,7,7]. In that view, it becomes clear that the median of the values is 7 after accounting for sampling.

## Sampling Based on Event Content (Keys)

This dynamic sampling approach involves tuning the sample rate based on event payload. At a high level, that means choosing one or more fields in the set of events and designating a sample rate when a certain combination of values is seen. For example, you could partition events based on HTTP response codes and then assign sample rates to each response code. Doing that allows you to specify sampling conditions such as these:

- Events with *errors* are more important than those with *successes*.
- Events for *newly placed orders* are more important than those checking on *order status*.
- Events affecting *paying customers* are more important to keep than those for customers using the *free tier*.

With this approach, you can make the keys as simple (e.g., HTTP method) or complicated (e.g., concatenating the HTTP method, request size, and user-agent) as needed to select samples that can provide the most useful view possible into service traffic.

Sampling at a constant percentage rate based on event content alone works well when the key space is small (e.g., there are a finite number of HTTP methods: GET, POST, HEAD, etc.) *and* when the relative rates of given keys stay consistent (e.g., when you can assume errors are less frequent than successes). It’s worth noting that sometimes that assumption can be wrong, or it can be reversed in bad cases. For those situations, you should validate your assumptions and have a plan for dealing with event traffic spikes if the assumed conditions reverse.

## Combining per Key and Historical Methods

When the content of traffic is harder to predict, you can instead continue to identify a key (or combination of keys) for each incoming event, and then *also* dynamically adjust the sample rate for each key based on the volume of traffic recently seen for that key. For example, base the sample rate on the number of times a given key combination (such as [customer ID, dataset ID, error code]) is seen in the last 30 seconds. If a specific combination is

the event. In *head-based sampling* (or *up-front sampling*), a sampling decision is made when the trace event is initiated. That decision is then propagated further downstream (e.g., by inserting a “require sampling” header bit) to ensure that every span necessary to complete the trace is sampled.

Some fields, like return status or latency, are known only in retrospect after event execution has completed. If a sampling decision relies on dynamic fields, by the time those are determined, each underlying service will have already independently chosen whether to sample other span events. At best, you may end up keeping the downstream spans deemed interesting outliers, but none of the other context. Properly making a decision on values known only at the end of a request requires *tail-based sampling*.

To collect full traces in the tail-based approach, all spans must first be collected in a buffer and then, retrospectively, a sampling decision can be made. That buffered sampling technique is computationally expensive and not feasible in practice entirely from within the instrumented code. Buffered sampling techniques typically require external collector-side logic.

Additional nuances exist for determining the *what* and *when* of sampling decisions. But at this point, those are best explained using code-based examples.

## Translating Sampling Strategies into Code

So far, we’ve covered sampling strategies on a conceptual level. Let’s look at how these strategies are implemented in code. This pedagogical example uses Go to illustrate implementation details, but the examples would be straightforward to port into any language that supports hashes/dicts/maps, pseudorandom number generation, and concurrency/timers.

## The Base Case

Let’s suppose you would like to instrument a high-volume handler that calls a downstream service, performs some internal work, and then returns a
