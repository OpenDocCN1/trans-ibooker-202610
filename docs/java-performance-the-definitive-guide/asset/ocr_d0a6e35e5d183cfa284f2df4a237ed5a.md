![](bbox=[0.141, 0.081, 0.858, 0.397])

*Figure 5-1. Idealized GC heap during collection*

It is simpler to perform these operations if no application threads are running while the garbage collector is running. Java programs are typically heavily multithreaded, and the garbage collector itself often runs multiple threads. This discussion considers two logical groups of threads: those performing application logic (often called *mutator threads*, since they are mutating objects as part of the application logic) and those performing GC. When GC threads track object references or move objects around in memory, they must make sure that application threads are not using those objects. This is particularly true when GC moves objects around: the memory location of the object changes during that operation, and hence no application threads can be accessing the object.

The pauses when all application threads are stopped are called *stop-the-world pauses*. These pauses generally have the greatest impact on the performance of an application, and minimizing those pauses is one important consideration when tuning GC.

# Generational Garbage Collectors

Though the details differ somewhat, most garbage collectors work by splitting the heap into generations. These are called the *old (or tenured) generation* and the *young generation*. The young generation is further divided into sections known as *eden* and the *survivor spaces* (though sometimes, *eden* is incorrectly used to refer to the entire young generation).