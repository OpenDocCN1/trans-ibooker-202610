was moved from the young generation into regions in the old generation). This is how G1 GC compacts the old generation—moving the objects like this is essentially compacting the heap as G1 GC goes along.

![](bbox=[0.125, 0.145, 0.815, 0.36])

Figure 6-6. Mixed GC performed by G1 GC

The mixed GC operation usually looks like this in the log:

```
79.826: [GC pause (mixed), 0.26161600 secs]
....
 [Eden: 1222M(1222M)->0B(1220M)
 Survivors: 142M->144M Heap: 3200M(4096M)->1964M(4096M)]
 [Times: user=1.01 sys=0.00, real=0.26 secs]
```

```
[3.800s][info][gc,start    ] GC(24) Pause Young (Mixed) (G1 Evacuation Pause)
[3.800s][info][gc,task     ] GC(24) Using 4 workers of 4 for evacuation
[3.800s][info][gc,phases   ] GC(24)  Pre Evacuate Collection Set: 0.2ms
[3.825s][info][gc,phases   ] GC(24) Evacuate Collection Set: 250.3ms
[3.826s][info][gc,phases   ] GC(24) Post Evacuate Collection Set: 0.3ms
[3.826s][info][gc,phases   ] GC(24) Other: 0.4ms
[3.826s][info][gc,heap     ] GC(24) Eden regions: 1222->0(1220)
[3.826s][info][gc,heap     ] GC(24) Survivor regions: 142->144(144)
[3.826s][info][gc,heap     ] GC(24) Old regions: 1834->1820
[3.826s][info][gc,heap     ] GC(24) Humongous regions: 4->4
[3.826s][info][gc,metaspace] GC(24) Metaspace: 3750K->3750K(1056768K)
[3.826s][info][gc         ] GC(24) Pause Young (Mixed) (G1 Evacuation Pause)
 3791M->3791M(3983M) 124.390ms
[3.826s][info][gc,cpu      ] GC(24) User=1.01s Sys=0.00s Real=0.26s
[3.826s][info][gc,start    ] GC(25) Pause Young (Mixed) (G1 Evacuation Pause)
```

Notice that the entire heap usage has been reduced by more than just the 1,222 MB removed from eden. That difference (16 MB) seems small, but remember that some of the survivor space was promoted into the old generation at the same time; in addition, each mixed GC cleans up only a portion of the targeted old generation regions.