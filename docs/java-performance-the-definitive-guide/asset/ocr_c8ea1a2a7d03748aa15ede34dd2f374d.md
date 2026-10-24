In the next chapter, which details how the various algorithms work, the reasons behind these points will be clearer (as well as ways to tune the collectors around them). For now, we’ll look at a few examples that prove the point.

First, let’s look at the data in Table 5-7. This test is the same code we used before for batch jobs with long calculations, though it has a few modifications: multiple application threads are doing calculations (two, in this case), the old generation is seeded with objects to keep it 65% full, and almost all objects can be collected directly from the young generation. This test is run on a system with four CPUs (not hyper-threaded) so that there is sufficient CPU for the G1 GC background threads to run.

# Table 5-7. Batch processing with long-lived objects

| Metric | G1 GC | Throughput |
|---|---|---|
| Elapsed time | 212 seconds | 193 seconds |
| CPU usage | 67% | 51% |
| Young GC pauses | 30 seconds | 13.5 seconds |
| Full GC pauses | 0 seconds | 1.5 seconds |

Because so few objects are promoted to the old generation, the throughput collector paused the application threads for only 15 seconds, and only 1.5 seconds of that was to collect the old generation.

Although the old generation doesn’t get many new objects promoted into it, the test seeds the old generation such that the G1 GC threads will scan it for garbage. This makes more work for the background GC threads, and it causes G1 GC to perform more work collecting the young generation in an attempt to compensate for the fuller old generation. The end result is that G1 GC paused the application for 30 seconds during the two-thread test—more than the throughput collector did.

Another example: when there isn’t sufficient CPU for the G1 GC background thread to run, the throughput collector will perform better, as Table 5-8 shows.

# Table 5-8. Batch processing with busy CPUs

| Metric | G1 GC | Throughput |
|---|---|---|
| Elapsed time | 287 seconds | 267 seconds |
| CPU usage | 99% | 99% |
| Young GC pauses | 80 seconds | 63 seconds |
| Full GC pauses | 0 seconds | 37 seconds |