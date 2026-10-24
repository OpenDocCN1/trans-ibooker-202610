613 MB. The operation took less than 0.02 seconds (the 0.02 seconds of real time at the end of the output is 0.0169320 seconds—the actual time—rounded). The program was charged for more CPU time than real time because the young collection was done by multiple threads (in this configuration, four threads).

The same log in JDK 11 would look something like this:

```
```text
[17.805s][info][gc,start       ] GC(4) Pause Young (Allocation Failure)
[17.806s][info][gc,heap        ] GC(4) PSYoungGen: 227983K->14463K(264128K)
[17.806s][info][gc,heap        ] GC(4) ParOldGen: 28012K->56610K(613696K)
[17.806s][info][gc,metaspace   ] GC(4) Metaspace: 3743K->3743K(1056768K)
[17.806s][info][gc             ] GC(4) Pause Young (Allocation Failure)
                                      496M->79M(857M) 16.932ms
[17.086s][info][gc,cpu        ] GC(4) User=0.05s Sys=0.00s Real=0.02s
```
```

The information here is the same; it’s just a different format. And this log entry has multiple lines; the previous log entry is actually a single line (but that doesn’t reproduce in this format). This log also prints out the metaspace sizes, but those will never change during a young collection. The metaspace is also not included in the total heap size reported on the fifth line of this sample.

Figure 6-2 shows the heap before and after a full GC.

![](bbox=[0.143, 0.444, 0.858, 0.676])

The old collection frees everything out of the young generation. The only objects that remain in the old generation are those that have active references, and all of those objects have been compacted so that the beginning of the old generation is occupied, and the remainder is free.

The GC log reports that operation like this:

```
```text
64.546: [Full GC (Ergonomics) [PSYoungGen: 15808K->0K(339456K)]
          [ParOldGen: 457753K->392528K(554432K) 473561K->392528K(893888K)
          [Metaspace: 56728K->56728K(115392K)], 1.3367080 secs]
          [Times: user=4.44 sys=0.01, real=1.34 secs]
```
```