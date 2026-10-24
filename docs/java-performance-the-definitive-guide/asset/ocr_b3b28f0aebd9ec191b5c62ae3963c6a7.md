concurrent compacting collectors have that same advantage over the throughput collector, and a similar (but smaller) advantage over G1 GC.

# No Collection: Epsilon GC

JDK 11 also contains a collector that does nothing: the *epsilon collector*. When you use this collector, objects are never freed from the heap, and when the heap fills up, you will get an out-of-memory error.

Traditional programs will not be able to use this collector, of course. It is really designed for internal JDK testing but can conceivably be useful in two situations:

+   - Very short-lived programs
+   - Programs carefully written to reuse memory and never perform new allocations

That second category is useful in some embedded environments with limited memory. That sort of programming is specialized; we won’t consider it here. But the first case holds interesting possibilities.

Consider the case of a program that allocates an array list of 4,096 elements, each of which is a 0.5 MB byte array. The time to run that program with various collectors is shown in Table 6-6. Default GC tunings are used in this example.

*Table 6-6. Performance metrics of a small allocation-based program*

| Collector | Time | Heap required |
|---|---|---|
| Throughput GC | 2.3 s | 3,072 MB |
| G1 GC | 3.24 s | 4,096 MB |
| Epsilon | 1.6 s | 2,052 MB |

Disabling garbage collection is a significant advantage in this case, yielding a 30% improvement. And the other collectors require significant memory overhead: like the other experimental collectors we’ve seen, the epsilon collector is not generational (because the objects cannot be freed, there’s no need to set up a separate space to be able to free them quickly). So for this test that produces an object of about 2 GB, the total heap required for the epsilon collector is just over that; we can run that case with -Xmx2052m. The throughput collector needs one-third more memory to hold its young generation, while G1 GC needs even more memory to set up all its regions.

To use this collector, you again specify the -XX:+UnlockExperimentalVMOptions flag with -XX:+UseEpsilonGC.

Running with this collector is risky unless you are certain that the program will never need more memory than you provide it. But in those cases, it can give a nice performance boost.