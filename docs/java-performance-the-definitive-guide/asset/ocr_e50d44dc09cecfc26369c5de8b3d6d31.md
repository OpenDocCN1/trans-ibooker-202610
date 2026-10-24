The indefinite reference object itself now has (at least) two strong references to it: the original strong reference(s) created by the application, and a new strong reference (created by the JVM) on a reference queue. All of these strong references must be cleared before the indefinite reference object itself can be reclaimed by the garbage collector.

Typically, this cleanup is done by whatever code is processing the reference queue. That code will get notified that a new object is on the queue and immediately remove all strong references to that object. Then, during the next GC cycle, the indefinite reference object (referent) will get freed. In the worst case, that reference queue will not be processed immediately, and there can be many GC cycles before everything is cleaned up. Even in the best case, though, the indefinite reference has to go through two GC cycles before it is freed.

Depending on the type of indefinite reference, some important variations to this general algorithm exist, but all indefinite references have this penalty to some degree.

# GC Logs and Reference Handling

When running an application that uses a lot of indefinite references, consider adding the -XX:+PrintReferenceGC flag (which is false by default). This allows you to see how much time is spent processing those references:

```
[GC[SoftReference, 0 refs, 0.0000060 secs]
[WeakReference, 238425 refs, 0.0236510 secs]
[FinalReference, 4 refs, 0.0000160 secs]
[PhantomReference, 0 refs, 0.0000010 secs]
[JNI Weak Reference, 0.0000020 secs]
[PSYoungGen: 271630K->17566K(305856K)]
271630K->17566K(1004928K), 0.0797140 secs]
[Times: user=0.16 sys=0.01, real=0.08 secs]
```

In this case, the use of 238,425 weak references added 23 ms to the young collection.

# Soft references

Soft references are used when the object in question has a good chance of being reused in the future, but you want to let the garbage collector reclaim the object if it hasn’t been used recently (a calculation that also takes into consideration the amount of memory the heap has available). Soft references are essentially one large, least recently used (LRU) object pool. The key to getting good performance from them is to make sure that they are cleared on a timely basis.

Here is an example. The stock server can set up a global cache of stock histories keyed by their symbol (or symbol and date). When a request comes in for the stock history