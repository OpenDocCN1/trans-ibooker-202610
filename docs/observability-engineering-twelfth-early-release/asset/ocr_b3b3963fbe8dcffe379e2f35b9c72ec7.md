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

func checkSampleRate(resp http.ResponseWriter, start time.Time,
err error,
    sampleRates map[interface{}]float64, counts
map[interface{}]int) float64 {
    msg := ""
    if err != nil {
        msg = err.Error()
    }
    roundedLatency := 100 *(time.Since(start) /
(100*time.Millisecond))
    k := SampleKey {
        ErrMsg:       msg,
        BackendShard: resp.Header().Get("Backend-Shard"),
        LatencyBucket: roundedLatency,
    }
    if neverSample(k) {
        return -1.0
    }

    counts[k]++
    if r, ok := sampleRates[k]; ok {
```