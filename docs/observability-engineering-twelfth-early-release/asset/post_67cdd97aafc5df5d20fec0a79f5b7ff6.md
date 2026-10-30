
(for example, has latency above 500 ms or is an error), it can be designated for tail sampling at its own guaranteed rate, while rate-limiting the other requests to fit within a budget of sampled requests per second:

```
var targetEventsPerSec = flag.Int("targetEventsPerSec", 4,
    "The target number of ordinary requests per second to sample
from this service.")
var outlierEventsPerSec = flag.Int("outlierEventsPerSec", 1,
    "The target number of outlier requests per second to sample
from this service.")

var sampleRate float64 = 1.0
var requestsInPastMinute *int

var outlierSampleRate float64 = 1.0
var outliersInPastMinute *int

func main() {
    // Initialize counters.
    rc := 0
    requestsInPastMinute = &rc
    oc := 0
    outliersInPastMinute = &oc

    go func() {
        for {
            time.Sleep(time.Minute)
            newSampleRate = *requestsInPastMinute / (60 * *targetEventsPerSec)
            if newSampleRate < 1 {
                sampleRate = 1.0
            } else {
                sampleRate = newSampleRate
            }
            newRequestCounter := 0
            requestsInPastMinute = &newRequestCounter

            newOutlierRate = outliersInPastMinute / (60 * *outlierEventsPerSec)
            if newOutlierRate < 1 {
                outlierSampleRate = 1.0
            } else {
                outlierSampleRate = newOutlierRate
            }
            newOutlierCounter := 0
            outliersInPastMinute = &newOutlierCounter
```

each key to make sampling decisions. Doing so modifies our example code like so:

```
var counts map[SampleKey]int
var sampleRates map[SampleKey]float64
var targetRates map[SampleKey]int

func neverSample(k SampleKey) bool {
    // Left to your imagination. Could be a situation where we
know request is a keepalive we never want to record, etc.
    return false
}

// Boilerplate main() and goroutine init to overwrite maps and
roll them over every interval goes here.

type SampleKey struct {
    ErrMsg        string
    BackendShard  int
    LatencyBucket int
}

// This might compute for each k: newRate[k] = counts[k] /
(interval * targetRates[k]), for instance.

func checkSampleRate(resp http.ResponseWriter, start time.Time, err error,
    sampleRates map[interface{}]float64, counts map[interface{}]int) float64 {
    msg := ""
    if err != nil {
        msg = err.Error()
    }
    roundedLatency := 100 * (time.Since(start) / (100*time.Millisecond))
    k := SampleKey {
        ErrMsg:        msg,
        BackendShard:  resp.Header().Get("Backend-Shard"),
        LatencyBucket: roundedLatency,
    }
    if neverSample(k) {
        return -1.0
    }

    counts[k]++
    if r, ok := sampleRates[k]; ok {
        return r
    } else {
        return 1.0
    }
}

func handler(resp http.ResponseWriter, req *http.Request) {
    var r float64
    if r, err := FloatFromHexBytes(req.Header.Get("Sampling-ID")); err != nil {
        r = rand.Float64()
    }

    start := time.Now()
    i, err := callAnotherService(r)
    resp.Write(i)

    sampleRate := checkSampleRate(resp, start, err, sampleRates, counts)
    if sampleRate > 0 && r < 1.0 / sampleRate {
        RecordEvent(req, sampleRate, start, err)
    }
}
```

At this point, our code example is becoming quite large, and it still lacks more-sophisticated techniques. Our example has been used to illustrate how sampling concepts are implemented.

Luckily, existing code libraries can handle this type of complex sampling logic. For Go, the *dynsample-go* library maintains a map over any number of sampling keys, allocating a fair share of sampling to each key as long as it is novel. That library also contains more-advanced techniques of computing sample rates, either based on target rates or without explicit target rates at all.

For this chapter, we’re close to having put together a complete introductory tour of applying sampling concepts. Before concluding, let’s make one last improvement by combining the tail-based sampling you’ve done so far with head-based sampling that can request tracing be sampled by all downstream services.

### Putting It All Together: Head and Tail per Key Target Rate Sampling

Earlier in this chapter, we noted that head-based sampling requires setting a header to propagating a sampling decision downstream. For the code example we’ve been iterating, that means the parent span must pass both the head-sampling decision and its corresponding rate to all child spans. Doing so forces sampling to occur for all child spans, even if the dynamic sampling rate at that level would not have chosen to sample the request:

```
var headCounts, tailCounts map[interface{}]int
var headSampleRates, tailSampleRates map[interface{}]float64

// Boilerplate main() and goroutine init to overwrite maps and
roll them over every interval goes here. checkSampleRate() etc.
from above as well

func handler(resp http.ResponseWriter, req *http.Request) {
    var r, upstreamSampleRate, headSampleRate float64
    if r, err := floatFromHexBytes(req.Header.Get("Sampling-ID")); err != nil {
        r = rand.Float64()
    }

    // Check if we have a non-negative upstream sample rate; if so, use it.
    if upstreamSampleRate, err := floatFromHexBytes(
        req.Header.Get("Upstream-Sample-Rate")); err == nil && upstreamSampleRate > 1.0 {
        headSampleRate = upstreamSampleRate
    } else {
        headSampleRate := checkHeadSampleRate(req, headSampleRates, headCounts)
        if headSampleRate > 0 && r < 1.0 / headSampleRate {
            // We'll sample this when recording event below; propagate the decision downstream though.
        } else {
            // Clear out headSampleRate as this event didn't qualify for sampling. This is a sentinel value.
            headSampleRate = -1.0
        }
    }

    start := time.Now()
```

become less likely that you would need to reimplement these sampling strategies in your own code.

However, even if you rely on third-party libraries to manage that strategy for you, it is essential that you understand the mechanics behind how sampling is implemented so you can understand which method is right for your particular situation. Understanding how the strategies (static versus dynamic, head versus tail, or a combination thereof) work in practice enables you to use them in a wise manner to achieve data fidelity while also optimizing for resource constraints.

Similar to deciding what and how to instrument your code, deciding *what*, *when*, and *how* to sample is best defined by your unique organizational needs. The fields in your events that influence how interesting they are to sample largely depend on how useful they are to understanding the state of your environment and their impact on achieving your business goals.

In the next chapter, we’ll examine an approach to routing large volumes of telemetry data: telemetry management with pipelines.

## Chapter 18. Telemetry Management with Pipelines

> **A NOTE FOR EARLY RELEASE READERS**
> 
> With Early Release ebooks, you get books in their earliest form—the authors’ raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.
> 
> This will be the 18th chapter of the final book.
> 
> If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at [vwillson@oreilly.com](mailto:vwillson@oreilly.com).

*This chapter is contributed by Suman Karumuri, senior staff software engineer at Slack, and Ryan Katkov, director of engineering at Slack*

### A NOTE FROM CHARITY, LIZ, AND GEORGE

In this part of the book, we’ve unpacked observability concepts that are most acutely felt at scale but that can be helpful at any scale for a variety of reasons. In the previous chapter, we looked at what happens when the typical trickle of observability data instead becomes a flood and how to reduce that volume with sampling. In this chapter, we’ll look at a different way to manage large volumes of telemetry data: with pipelines.

Beyond data volume, *telemetry pipelines* can help manage application complexity. In simpler systems, the telemetry data from an application can be directly sent to the appropriate data backend. In more-complex systems, you may need to route telemetry data to many backend systems in order to isolate workloads, meet security and compliance needs, satisfy different retention requirements, or for a variety of other reasons. When you add data volume on top of that, managing the many-to-many relationship between telemetry producers and consumers of that data can be extraordinarily complex. Telemetry pipelines help you abstract away that complexity.

This chapter, written by Suman Karumuri and Ryan Katkov, details Slack’s use of telemetry pipelines to manage its observability data. Similar to the other guest-contributed chapter in this book, Chapter 14, Slack’s application infrastructure is a wonderful example of elegantly tackling issues of complexity and scale in ways that surface helpful concepts to engineers using observability at any scale. We’re delighted to be able to share these lessons with you in the context of this book.

The rest of this chapter is told from the perspective of Suman and Ryan.

We’re happy to share this chapter on setting up and managing your telemetry streams with pipelines. In this chapter, we will go over how telemetry pipelines can benefit your organization’s observability capabilities, describe the basic structure and components of a telemetry pipeline, and show concrete examples of how Slack uses a telemetry
