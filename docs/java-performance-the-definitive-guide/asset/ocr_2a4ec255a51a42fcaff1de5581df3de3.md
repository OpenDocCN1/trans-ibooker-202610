# Promotion failure

G1 GC has completed a marking cycle and has started performing mixed GCs to clean up the old regions. Before it can clean enough space, too many objects are promoted from the young generation, and so the old generation still runs out of space. In the log, a full GC immediately follows a mixed GC:

```
2226.224: [GC pause (mixed)
    2226.440: [SoftReference, 0 refs, 0.000060 secs]
    2226.441: [WeakReference, 0 refs, 0.000020 secs]
    2226.441: [FinalReference, 0 refs, 0.000010 secs]
    2226.441: [PhantomReference, 0 refs, 0.0000010 secs]
    2226.441: [JNI Weak Reference, 0.000030 secs]
        (to-space exhausted), 0.2390040 secs]
....
    [Eden: 0.0B(400.0M)->0.0B(400.0M)
    Survivors: 0.0B->0.0B Heap: 2006.4M(2048.0M)->2006.4M(2048.0M)]
    [Times: user=1.70 sys=0.04, real=0.26 secs]
2226.510: [Full GC (Allocation Failure)
    2227.519: [SoftReference, 4329 refs, 0.0005520 secs]
    2227.520: [WeakReference, 12646 refs, 0.0010510 secs]
    2227.521: [FinalReference, 7538 refs, 0.0005660 secs]
    2227.521: [PhantomReference, 168 refs, 0.0000120 secs]
    2227.521: [JNI Weak Reference, 0.000020 secs]
        2006M->907M(2048M), 4.1615450 secs]
    [Times: user=6.76 sys=0.01, real=4.16 secs]

[2226.224s][info][gc      ] GC(26) Pause Young (Mixed)
                    (G1 Evacuation Pause)
                    2048M->2006M(2048M) 26.129ms
...
[2226.510s][info][gc,start    ] GC(27) Pause Full (G1 Evacuation Pause)
```

This failure means the mixed collections need to happen more quickly; each young collection needs to process more regions in the old generation.

# Evacuation failure

When performing a young collection, there isn’t enough room in the survivor spaces and the old generation to hold all the surviving objects. This appears in the GC logs as a specific kind of young GC:

```
60.238: [GC pause (young) (to-space overflow), 0.4154690 secs]

[60.238s][info][gc,start      ] GC(28) Pause Young (Concurrent Start)
                    (G1 Evacuation Pause)
[60.238s][info][gc,task       ] GC(28) Using 4 workers of 4
                    for evacuation
[60.238s][info][gc            ] GC(28) To-space exhausted
```