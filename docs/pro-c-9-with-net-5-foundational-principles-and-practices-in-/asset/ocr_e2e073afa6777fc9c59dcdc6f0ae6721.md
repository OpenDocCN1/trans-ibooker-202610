```
// Print out generation of refToMyCar object.
Console.WriteLine("Generation of refToMyCar is: {0}",
    GC.GetGeneration(refToMyCar));
Console.ReadLine();
```

After running this, you should see output similar to this:

```
**** Fun with System.GC ****

Estimated bytes on heap: 75760
This OS has 3 object generations.

Zippy is going 100 MPH
Generation of refToMyCar is: 0
```

You will explore more of the methods from Table 9-2 in the next section.

# Forcing a Garbage Collection

Again, the whole purpose of the garbage collector is to manage memory on your behalf. However, in some rare circumstances, it may be beneficial to programmatically force a garbage collection using GC.Collect(). Here are two common situations where you might consider interacting with the collection process:

+   - Your application is about to enter a block of code that you don’t want interrupted by a possible garbage collection.
+   - Your application has just finished allocating an extremely large number of objects, and you want to remove as much of the acquired memory as soon as possible.

If you determine it could be beneficial to have the garbage collector check for unreachable objects, you could explicitly trigger a garbage collection, as follows:

```
...
// Force a garbage collection and wait for
// each object to be finalized.
GC.Collect();
GC.WaitForPendingFinalizers();
...
```

When you manually force a garbage collection, you should always make a call to GC.WaitForPendingFinalizers(). With this approach, you can rest assured that all finalizable objects (described in the next section) have had a chance to perform any necessary cleanup before your program continues. Under the hood, GC.WaitForPendingFinalizers() will suspend the calling thread during the collection process. This is a good thing, as it ensures your code does not invoke methods on an object currently being destroyed!

The GC.Collect() method can also be supplied a numerical value that identifies the oldest generation on which a garbage collection will be performed. For example, to instruct the runtime to investigate only generation 0 objects, you would write the following: