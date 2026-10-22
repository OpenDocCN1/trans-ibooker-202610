![](bbox=[0.18, 0.08, 0.71, 0.48])

Figure 4-7. Barrier Synchronization

Java provides three primitives for barrier synchronization. I’ll show here how just one of the three, namely the CountDownLatch, works. The basic concepts apply to other barrier synchronization primitives.

When you create a CountDownLatch, you pass a value to its constructor that represents the number of threads that must block at the barrier before they are all allowed to continue. This is called in the thread which is managing the barrier points for the system – in Figure 4-7 this would be main().

```
CountDownLatch nextPhaseSignal = new CountDownLatch(numThreads);
```

Next you create the worker threads that will perform some actions and then block at the barrier until they all complete. To do this, you need to pass each thread a reference to CountDownLatch.

```
for (int i = 0; i < numThreads; i++) {
    Thread worker = new Thread(new
```