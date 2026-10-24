This is really no different than the case with a single CPU: the competition for CPU cycles between the G1 GC background threads and the application threads means that the application threads were effectively paused even when GC pauses weren’t happening.

If we’re more interested in interactive processing and response times, the throughput collector has a harder time beating G1 GC. If your server is short of CPU cycles such that the G1 GC and application threads compete for CPU, then G1 GC will yield worse response times (similar to the cases we’ve already seen). If the server is tuned such that there are no full GCs, then G1 GC and the throughput collector will generally turn out similar results. But the more full GCs that the throughput collector has, the better the G1 GC average, 90th%, and 99th% response times will be.

# Average CPU Usage and GC

Looking at only the average CPU during a test misses the interesting picture of what happens during GC cycles. The throughput collector will (by default) consume 100% of the CPU available on the machine while it runs, so a more accurate representation of the CPU usage during the test with two application threads is shown in Figure 5-2.

Most of the time, only the application threads are running, consuming 50% of the total CPU. When GC kicks in, 100% of the CPU is consumed. Hence, the actual CPU usage resembles the sawtooth pattern in the graph, even though the average during the test is reported as the value of the straight dashed line.

![](bbox=[0.201, 0.53, 0.778, 0.781])

*Figure 5-2. Actual versus average CPU usage (throughput)*

The effect is different in a concurrent collector, when background threads are running concurrently with the application threads. In that case, a graph of the CPU looks like Figure 5-3.