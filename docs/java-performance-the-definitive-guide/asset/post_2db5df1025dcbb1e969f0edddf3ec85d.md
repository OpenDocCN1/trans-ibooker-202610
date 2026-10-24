
![](img/1711f142ae3cfd740ef106afbbba2046_178_0.png)

This diagram presents three important things to observe. First, the young generation has changed its occupancy: there will be at least one (and possibly more) young collections during the concurrent cycle. Hence, the eden regions before the marking cycle have been completely freed, and new eden regions have started to be allocated.

Second, some regions are now marked with an X. Those regions belong to the old generation (and note that they still contain data)—they are regions that the marking cycle has determined contain mostly garbage.

Finally, notice that the old generation (consisting of the regions marked with an O or an X) is actually more occupied after the cycle has completed. That’s because the young generation collections that occurred during the marking cycle promoted data into the old generation. In addition, the marking cycle doesn’t actually free any data in the old generation: it merely identifies regions that are mostly garbage. Data from those regions is freed in a later cycle.

The G1 GC concurrent cycle has several phases, some of which stop all application threads, and some of which do not. The first phase is called *initial-mark* (in JDK 8) or *concurrent start* (in JDK 11). That phase stops all application threads—partly because it also executes a young collection, and it sets up the next phases of the cycle.

In JDK 8, that looks like this:

```
50.541: [GC pause (G1 Evacuation pause) (young) (initial-mark), 0.27767100 secs]
    ... lots of other data ...
    [Eden: 1220M(1220M)->0B(1220M)
     Survivors: 144M->144M Heap: 3242M(4096M)->2093M(4096M)]
    [Times: user=1.02 sys=0.04, real=0.28 secs]
```

And in JDK 11:

```
[50.261s][info   ][gc,start      ] GC(11) Pause Young (Concurrent Start)
                                      (G1 Evacuation Pause)
[50.261s][info   ][gc,task       ] GC(11) Using 4 workers of 4 for evacuation
```

Also note that in the JDK 11 example, the output has the same GC entry—20—as did the entry where the root region scanning occurred. We are breaking down the operations more finely than the JDK logging does: in the JDK, the entire background scanning is considered one operation. We're splitting the discussion into more fine-grained, logical operations, since, for example, the root scanning can introduce a pause when the concurrent marking cannot.

The marking phase is followed by a remarking phase and a normal cleanup phase:

```
120.910: [GC remark 120.959:
    [GC ref-PRC, 0.0000890 secs], 0.0718990 secs]
[Times: user=0.23 sys=0.01, real=0.08 secs]
120.985: [GC cleanup 3510M->3434M(4096M), 0.0111040 secs]
    [Times: user=0.04 sys=0.00, real=0.01 secs]

[120.909s][info][gc,start      ] GC(20) Pause Remark
[120.909s][info][gc,stringtable] GC(20) Cleaned string and symbol table,
                            strings: 1369 processed, 0 removed
                            symbols: 17173 processed, 0 removed
[120.985s][info][gc            ] GC(20) Pause Remark 2283M->862M(3666M) 80.412ms
[120.985s][info][gc,cpu        ] GC(20) User=0.23s Sys=0.01s Real=0.08s
```

These phases stop the application threads, though usually for a short time. Next an additional cleanup phase happens concurrently:

```
120.996: [GC concurrent-cleanup-start]
120.996: [GC concurrent-cleanup-end, 0.0004520]

[120.878s][info][gc,start      ] GC(20) Pause Cleanup
[120.879s][info][gc            ] GC(20) Pause Cleanup 1313M->1313M(3666M) 1.192ms
[120.879s][info][gc,cpu        ] GC(20) User=0.00s Sys=0.00s Real=0.00s
[120.879s][info][gc,marking    ] GC(20) Concurrent Cleanup for Next Mark
[120.996s][info][gc,marking    ] GC(20) Concurrent Cleanup for Next Mark
                            117.168ms
[120.996s][info][gc            ] GC(20) Concurrent Cycle 70,177.506ms
```

And with that, the normal G1 GC background marking cycle is complete—insofar as finding the garbage goes, at least. But very little has actually been freed yet. A little memory was reclaimed in the cleanup phase, but all G1 GC has really done at this point is to identify old regions that are mostly garbage and can be reclaimed (the ones marked with an X in Figure 6-5).

Now G1 GC executes a series of mixed GCs. They are called mixed because they perform the normal young collection but also collect some of the marked regions from the background scan. The effect of a mixed GC is shown in Figure 6-6.

As is usual for a young collection, G1 GC has completely emptied eden and adjusted the survivor spaces. Additionally, two of the marked regions have been collected. Those regions were known to contain mostly garbage, so a large part of them was freed. Any live data in those regions was moved to another region (just as live data was moved from the young generation into regions in the old generation). This is how G1 GC compacts the old generation—moving the objects like this is essentially compacting the heap as G1 GC goes along.

![](img/1711f142ae3cfd740ef106afbbba2046_182_0.png)

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

As we continue, you’ll see that it is important to make sure that the mixed GCs clean up enough memory to prevent future concurrent failures.

In JDK 11, the first mixed GC is labeled `Prepared Mixed` and immediately follows the concurrent cleanup.

The mixed GC cycles will continue until (almost) all of the marked regions have been collected, at which point G1 GC will resume regular young GC cycles. Eventually, G1 GC will start another concurrent cycle to determine which regions in the old generation should be freed next.

Although a mixed GC cycle usually says `(Mixed)` for the GC cause, the young collections are sometimes labeled normally following a concurrent cycle (i.e., G1 Evacuation Pause). If the concurrent cycle found regions in the old generation that can be completely freed, those regions are reclaimed during the regular young evacuation pause. Technically, this is not a mixed cycle in the implementation of the collector. Logically, though, it is: objects are being freed from the young generation or promoted into the old generation, and at the same time garbage objects (regions, really) are being freed from the old generation.

If all goes well, that’s the entire set of GC activities you’ll see in your GC log. But there are some failure cases to consider.

Sometimes you’ll observe a full GC in the log, which is an indication that more tuning (including, possibly, more heap space) will benefit the application performance. This is triggered primarily four times:

## *Concurrent mode failure*

G1 GC starts a marking cycle, but the old generation fills up before the cycle is completed. In that case, G1 GC aborts the marking cycle:

```
51.408: [GC concurrent-mark-start]
65.473: [Full GC 4095M->1395M(4096M), 6.1963770 secs]
        [Times: user=7.87 sys=0.00, real=6.20 secs]
71.669: [GC concurrent-mark-abort]

[51.408][info][gc,marking    ] GC(30) Concurrent Mark From Roots
...
[65.473][info][gc            ] GC(32) Pause Full (G1 Evacuation Pause)
                                      4095M->1305M(4096M) 60,196.377

...
[71.669][info][gc,marking    ] GC(30) Concurrent Mark From Roots 191ms
[71.669][info][gc,marking    ] GC(30) Concurrent Mark Abort
```

This failure means that heap size should be increased, the G1 GC background processing must begin sooner, or the cycle must be tuned to run more quickly (e.g., by using additional background threads). Details on how to do that follow.
