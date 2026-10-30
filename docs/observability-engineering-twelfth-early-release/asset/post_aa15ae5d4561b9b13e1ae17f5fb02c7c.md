
result and unconditionally records an event to your instrumentation sink:

```go
func handler(resp http.ResponseWriter, req *http.Request) {
    start := time.Now()
    i, err := callAnotherService()
    resp.Write(i)
    RecordEvent(req, start, err)
}
```

At scale, this instrumentation approach is unnecessarily noisy and would result in sky-high resource consumption. Let’s look at alternate ways of sampling the events this handler would send.

## Fixed-Rate Sampling

A naïve approach might be probabilistic sampling using a fixed rate, by randomly choosing to send 1 in 1,000 events:

```go
var sampleRate = flag.Int("sampleRate", 1000, "Static sample rate")

func handler(resp http.ResponseWriter, req *http.Request) {
    start := time.Now()
    i, err := callAnotherService()
    resp.Write(i)

    r := rand.Float64()
    if r < 1.0 / *sampleRate {
        RecordEvent(req, start, err)
    }
}
```

Every 1,000th event would be kept, regardless of its relevance, as representative of the other 999 events discarded. To reconstruct your data on the backend, you would need to remember that each event stood for `sampleRate` events and multiply out all counter values accordingly on the receiving end at the instrumentation collector. Otherwise, your tooling would misreport the total number of events actually encountered during that time period.

## Recording the Sample Rate

In the preceding clunky example, you would need to manually remember and set the sample rate at the receiving end. What if you need to change the sample rate value at some point in the future? The instrumentation collector wouldn’t know exactly when the value changed. A better practice is to explicitly pass the current `sampleRate` when sending a sampled event — indicating that the event statistically represents `sampleRate` similar events. Note that sample rates can vary not only between services, but also within a single service.

| Status code | Time | Sample-rate |
|---|---|---|
| ok | 1:00 | 100 |
| ok | 1:00 | 100 |
| err | 1:00 | 1 |
| ok | 1:01 | 80 |
| err | 1:01 | 1 |
| ok | 1:01 | 80 |
| ok | 1:01 | 80 |

*Figure 17-1. Different events may be sampled at different rates*

Recording the sample rate within an event can look like this:

| Status code | Time | COUNT (reweighted) |
|---|---|---|
| ok | 1:00 | 200 |
| err | 1:00 | 1 |
| ok | 1:01 | 240 |
| err | 1:01 | 1 |

*Figure 17-2. Total events can be calculated using weighted numbers*

This example is simplistic and contrived. Already, you may be seeing flaws in this approach when it comes to handling trace events. In the next section, we will look at additional considerations for making dynamic sampling rates and tracing work well together.

## Consistent Sampling

So far in our code, we’ve looked at how a sampling decision is made. But we have yet to consider when a sampling decision gets made in the case of sampling trace events. The strategy of using head-based, tail-based, or buffered sampling matters when considering how sampling interacts with tracing. We’ll cover how those decisions get implemented toward the end of the chapter. For now, let’s examine how to propagate context to downstream handlers in order to (later) make that decision.

To properly manage trace events, you should use a centrally generated sampling/tracing ID propagated to all downstream handlers instead of independently generating a sampling decision inside each one. Doing so lets you make consistent sampling decisions for different manifestations of the same end user’s request (see Figure 17-3). In other words, that ensures that

you capture a full end-to-end trace for any given sampled request. It would be unfortunate to discover that you have sampled an error far downstream for which the upstream context is missing because it was dropped because of how your sampling strategy was implemented.

| HASH (traceid) | Sampled? |
|---|---|
| 8464143 |  |
| 9976727 |  |
| 2046000 | YES |
| 8697994 |  |
| 1983000 | YES |
| 3427217 |  |
| 6152331 |  |
| 4919000 | YES |
| 6122453 |  |

*Figure 17-3. Sampled events containing a TraceId*

Consistent sampling ensures that when the sample rate is held constant, traces are either kept or sampled away in their entirety. And if children are sampled at a higher sample rate—for instance, noisy Redis calls being sampled 1 for 1,000, while their parents are kept 1 for 10—it will never be the case that a broken trace is created from a Redis child being kept while its parent is discarded.

Let’s modify the previous code sample to read a value for sampling probability from the TraceID/Sampling-ID, instead of generating a random value at each step:

```
var sampleRate = flag.Int("sampleRate", 1000, "Service's sample rate")

func handler(resp http.ResponseWriter, req *http.Request) {
    // Use an upstream-generated random sampling ID if it exists.
    // otherwise we're a root span. generate & pass down a random ID.
    var r float64
    if r, err := floatFromHexBytes(req.Header.Get("Sampling-ID")); err != nil {
        r = rand.Float64()
    }

    start := time.Now()
    // Propagate the Sampling-ID when creating a child span
    i, err := callAnotherService(r)
    resp.Write(i)

    if r < 1.0 / *sampleRate {
        RecordEvent(req, *sampleRate, start, err)
    }
}
```

Now, by changing the sampleRate feature flag to cause a different proportion of traces to be sampled, you have support for adjusting the sample rate without recompiling, including at runtime. However, if you adopt the technique we’ll discuss next, target rate sampling, you won’t need to manually adjust the rate.

## Target Rate Sampling

You don’t need to manually flag-adjust the sampling rates for each of your services as traffic swells and sags. Instead, you can automate this process by tracking the incoming request rate that you’re receiving (see Figure 17-4).

![](img/aaaa2d27e08e754554d501192f304d8f_320_0.png)

*Figure 17-4. You can automate the calculation of overall sample volume*

Let’s see how that is done in our code example:

```go
var targetEventsPerSec = flag.Int("targetEventsPerSec", 5,
	"The target number of requests per second to sample from this
service.")

// Note: sampleRate can be a float! doesn't have to be an
integer.
var sampleRate float64 = 1.0
// Track requests from previous minute to decide sampling rate
for the next minute.
var requestsInPastMinute *int

func main() {
	// Initialize counters.
	rc := 0
	requestsInPastMinute = &rc

	go func() {
		for {
```

percentile outlier event will be chosen for random sampling is slim. Likewise, you may want to have at least some data for each of your distinct sources, rather than have the high-volume sources drown out the low-volume ones.

A remedy for that scenario is to set more than one sample rate. Let’s start by varying the sample rates by key. Here, the example code samples any baseline (nonoutlier) events at a rate of 1 in 1,000 and chooses to tail-sample any errors or slow queries at a rate of 1 in 1 and 1 in 5, respectively:

```go
var sampleRate = flag.Int("sampleRate", 1000, "Service's sample rate")
var outlierSampleRate = flag.Int("outlierSampleRate", 5, "Outlier sample rate")

func handler(resp http.ResponseWriter, req *http.Request) {
    start := time.Now()
    i, err := callAnotherService(r)
    resp.Write(i)

    r := rand.Float64()
    if err != nil || time.Since(start) > 500*time.Millisecond {
        if r < 1.0 / *outlierSampleRate {
            RecordEvent(req, *outlierSampleRate, start, err)
        }
    } else {
        if r < 1.0 / *sampleRate {
            RecordEvent(req, *sampleRate, start, err)
        }
    }
}
```

Although this is a good example of using multiple static sample rates, the approach is still susceptible to spikes of instrumentation traffic. If the application experiences a spike in the rate of errors, every single error gets sampled. Next, we will address that shortcoming with target rate sampling.

## Sampling by Key and Target Rate

Putting two previous techniques together, let’s extend what we’ve already done to target specific rates of instrumentation. If a request is anomalous
