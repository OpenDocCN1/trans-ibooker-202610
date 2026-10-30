# Recording the Sample Rate

In the preceding clunky example, you would need to manually remember and set the sample rate at the receiving end. What if you need to change the sample rate value at some point in the future? The instrumentation collector wouldn’t know exactly when the value changed. A better practice is to explicitly pass the current `sampleRate` when sending a sampled event (see [Figure 17-1](#))—indicating that the event statistically represents `sampleRate` similar events. Note that sample rates can vary not only between services, but also within a single service.

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