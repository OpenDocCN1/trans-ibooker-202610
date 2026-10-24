This is an indication that the heap is largely full or fragmented. G1 GC will attempt to compensate, but you can expect this to end badly: the JVM will resort to performing a full GC. The easy way to overcome this is to increase the heap size, though possible solutions are given in “Advanced Tunings” on page 182.

# Humongous allocation failure

Applications that allocate very large objects can trigger another kind of full GC in G1 GC; see “G1 GC allocation of humongous objects” on page 192 for more details (including how to avoid it). In JDK 8, it isn’t possible to diagnose this situation without resorting to special logging parameters, but in JDK 11 that is shown with this log:

```
```text
[3023.091s][info][gc,start      ] GC(54) Pause Full (G1 Humongous Allocation)
```
```

# Metadata GC threshold

As I’ve mentioned, the metaspace is essentially a separate heap and is collected independently of the main heap. It is not collected via G1 GC, but still when it needs to be collected in JDK 8, G1 GC will perform a full GC (immediately preceded by a young collection) on the main heap:

```
```text
0.0535: [GC (Metadata GC Threshold) [PSYoungGen: 34113K->20388K(291328K)]
    73838K->60121K(794112K), 0.0282912 secs]
    [Times: user=0.05 sys=0.01, real=0.03 secs]
0.0566: [Full GC (Metadata GC Threshold) [PSYoungGen: 20388K->0K(291328K)]
    [ParOldGen: 39732K->46178K(584192K), 60121K->46178K(875520K),
    [Metaspace: 59040K->59036K(1101824K)], 0.1121237 secs]
    [Times: user=0.28 sys=0.01, real=0.11 secs]
```
```

In JDK 11, the metaspace can be collected/resized without requiring a full GC.

![](bbox=[0.168, 0.575, 0.252, 0.654])

# Quick Summary

+   - G1 has multiple cycles (and phases within the concurrent cycle). A well-tuned JVM running G1 should experience only young, mixed, and concurrent GC cycles.
+   - Small pauses occur for some of the G1 concurrent phases.
+   - G1 should be tuned if necessary to avoid full GC cycles.

# Tuning G1 GC

The major goal in tuning G1 GC is to make sure that no concurrent mode or evacuation failures end up requiring a full GC. The techniques used to prevent a full GC can also be used when frequent young GCs must wait for a root region scan to complete.