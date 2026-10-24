166, such as the one used for the examples of how to create and store canonical objects).

Using these classes is convenient, but be aware that they have two costs. First, as discussed throughout this section, indefinite references can have a negative effect on the garbage collector. Second, the class itself must periodically perform an operation to clear all the unreferenced data in the collection (i.e., that class is responsible for processing the reference queue of the indefinite references it stores).

The WeakHashMap class, for instance, uses weak references for its keys. When the weakly referenced key is no longer available, the WeakHashMap code must clear out the value in the map that used to be associated with that key. That operation is carried out every time the map is referenced: the reference queue for the weak key is processed, and the value associated with any key on the reference queue is removed from the map.

This has two performance implications. First, the weak reference and its associated value won’t actually be freed until the map is used again. So if the map is used infrequently, the memory associated with the map won’t be freed as quickly as desired.

Second, the performance of operations on the map is unpredictable. Normally, operations on a hash map are fast; that’s why the hash map is so popular. The operation on a WeakHashMap immediately after a GC will have to process the reference queue; that operation no longer has a fixed, short time. So even if the keys are freed somewhat infrequently, performance will be difficult to predict. Worse, if the keys in the map are freed quite frequently, the performance of the WeakHashMap can be quite bad.

Collections based on indefinite references can be useful, but they should be approached with caution. If it is feasible, have the application manage the collection itself.

# Finalizers and final references

Every Java class has a finalize() method inherited from the Object class; that method can be used to clean up data after the object is eligible for GC. That sounds like a nice feature, and it is required in a few circumstances. In practice, it turns out to be a bad idea, and you should try hard not to use this method.

Finalizers are so bad that the finalize() method is deprecated in JDK 11 (though not in JDK 8). We’ll get into the details of why finalizers are bad in the rest of this section, but first, a little motivation. Finalizers were originally introduced into Java to address problems that can arise when the JVM manages the life cycle of objects. In a language like C++, where you must explicitly destroy an object when you no longer need it, the destructor for the object could clean up the state of that object. In Java, when the object is automatically reclaimed as it goes out of scope, the finalizer served as the deconstructor.