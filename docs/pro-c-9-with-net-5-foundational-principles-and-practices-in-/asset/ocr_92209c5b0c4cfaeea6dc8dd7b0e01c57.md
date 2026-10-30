Note Strictly speaking, the garbage collector uses two distinct heaps, one of which is specifically used to store large objects. This heap is less frequently consulted during the collection cycle, given possible performance penalties involved with relocating large objects. In .NET Core, the large heap can be compacted on demand or when optional hard limits for absolute or percentage memory usage is reached.

# Understanding Object Generations

When the runtime is attempting to locate unreachable objects, it does not literally examine every object placed on the managed heap. Doing so, obviously, would involve considerable time, especially in larger (i.e., real-world) applications.

To help optimize the process, each object on the heap is assigned to a specific “generation.” The idea behind generations is simple: the longer an object has existed on the heap, the more likely it is to stay there. For example, the class that defined the main window of a desktop application will be in memory until the program terminates. Conversely, objects that have only recently been placed on the heap (such as an object allocated within a method scope) are likely to be unreachable rather quickly. Given these assumptions, each object on the heap belongs to a collection in one of the following generations:

+   - Generation 0: Identifies a newly allocated object that has never been marked for collection (with the exception of large objects, which are initially placed in a generation 2 collection). Most objects are reclaimed for garbage collection in generation 0 and do now survive to generation 1.
+   - Generation 1: Identifies an object that has survived a garbage collection. This generation also serves as a buffer between short-lived objects and long-lived objects.
+   - Generation 2: Identifies an object that has survived more than one sweep of the garbage collector or a significantly large object that started in a generation 2 collection.

Note Generations 0 and 1 are termed ephemeral generations. As explained in the next section, you will see that the garbage collection process does treat ephemeral generations differently.

The garbage collector will investigate all generation 0 objects first. If marking and sweeping (or said more plainly, getting rid of) these objects results in the required amount of free memory, any surviving objects are promoted to generation 1. To see how an object’s generation affects the collection process, ponder Figure 9-5, which diagrams how a set of surviving generation 0 objects (A, B, and E) are promoted once the required memory has been reclaimed.