# Target Rate Sampling

You don’t need to manually flag-adjust the sampling rates for each of your services as traffic swells and sags. Instead, you can automate this process by tracking the incoming request rate that you’re receiving (see [Figure 17-4](#)).

![](bbox=[0.145, 0.215, 0.863, 0.511])

*Figure 17-4. You can automate the calculation of overall sample volume*

Let’s see how that is done in our code example:

```
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
```