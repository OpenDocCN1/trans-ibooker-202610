The G1 GC young collection is triggered when eden fills up (in this case, after filling four regions). After the collection, eden is empty (though regions are assigned to it, which will begin to fill up with data as the application proceeds). At least one region is assigned to the survivor space (partially filled in this example), and some data has moved into the old generation.

The GC log illustrates this collection a little differently in G1 than in other collectors. The JDK 8 example log was taken using `PrintGCDetails`, but the details in the log for G1 GC are more verbose. The examples show only a few of the important lines.

Here is the standard collection of the young generation:

```
23.430: [GC pause (young), 0.23094400 secs]
...
    [Eden: 1286M(1286M)->0B(1212M)
     Survivors: 78M->152M Heap: 1454M(4096M)->242M(4096M)]
     [Times: user=0.85 sys=0.05, real=0.23 secs]
```

Collection of the young generation took 0.23 seconds of real time, during which the GC threads consumed 0.85 seconds of CPU time. 1,286 MB of objects were moved out of eden (which was adaptively resized to 1,212 MB); 74 MB of that was moved to the survivor space (it increased in size from 78 MB to 152 MB), and the rest were freed. We know they were freed by observing that the total heap occupancy decreased by 1,212 MB. In the general case, some objects from the survivor space might have been moved to the old generation, and if the survivor space were full, some objects from eden would have been promoted directly to the old generation—in those cases, the size of the old generation would increase.

The similar log in JDK 11 looks like this:

```
[23.200s][info   ][gc,start      ] GC(10) Pause Young (Normal)
                                            (G1 Evacuation Pause)
[23.200s][info   ][gc,task       ] GC(10) Using 4 workers of 4 for evacuation
[23.430s][info   ][gc,phases     ] GC(10) Pre Evacuate Collection Set: 0.0ms
[23.430s][info   ][gc,phases     ] GC(10) Evacuate Collection Set: 230.3ms
[23.430s][info   ][gc,phases     ] GC(10) Post Evacuate Collection Set: 0.5ms
[23.430s][info   ][gc,phases     ] GC(10) Other: 0.1ms
[23.430s][info   ][gc,heap       ] GC(10) Eden regions: 643->606(606)
[23.430s][info   ][gc,heap       ] GC(10) Survivor regions: 39->76(76)
[23.430s][info   ][gc,heap       ] GC(10) Old regions: 67->75
[23.430s][info   ][gc,heap       ] GC(10) Humongous regions: 0->0
[23.430s][info   ][gc,metaspace  ] GC(10) Metaspace: 18407K->18407K(1067008K)
[23.430s][info   ][gc             ] GC(10) Pause Young (Normal)
                                            (G1 Evacuation Pause)
                                            1454M(4096M)->242M(4096M) 230.104ms
[23.430s][info   ][gc,cpu        ] GC(10) User=0.85s Sys=0.05s Real=0.23s
```

A concurrent G1 GC cycle begins and ends as shown in Figure 6-5.