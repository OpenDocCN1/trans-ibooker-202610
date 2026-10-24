# JFR Memory view

The information gathered here is extensive. Figure 3-11 shows just one panel of the Memory view.

![](bbox=[0.14, 0.15, 0.86, 0.55])

Figure 3-11. Java Flight Recorder Memory panel

This graph shows that memory is fluctuating fairly regularly as the young generation is cleared (and we see that the heap overall is growing during this time: it started at about 340 MB and ends about at 2 GB). The lower-left panel shows all the collections that occurred during the recording, including their duration and type of collection (always a ParallelScavenge in this example). When one of those events is selected, the bottom-right panel breaks that down even further, showing all the specific phases of that collection and how long each took.

The various tabs on this page provide a wealth of other information: how long and how many reference objects were cleared, whether there are promotion or evacuation failures from the concurrent collectors, the configuration of the GC algorithm itself (including the sizes of the generations and the survivor space configurations), and even information on the specific kinds of objects that were allocated. As you read through Chapters 5 and 6, keep in mind how this tool can diagnose the problems that are discussed there. If you need to understand why the G1 GC collector bailed out and performed a full GC (was it due to promotion failure?), how the JVM has