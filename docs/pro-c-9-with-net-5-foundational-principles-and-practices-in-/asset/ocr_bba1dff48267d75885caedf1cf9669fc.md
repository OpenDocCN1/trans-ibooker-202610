# The Basics of Object Lifetime

When you are building your C# applications, you are correct to assume that the .NET Core runtime environment will take care of the managed heap without your direct intervention. In fact, the golden rule of .NET Core memory management is simple.

Rule Allocate a class instance onto the managed heap using the new keyword and forget about it.

Once instantiated, the garbage collector will destroy an object when it is no longer needed. The next obvious question, of course, is “How does the garbage collector determine when an object is no longer needed?” The short (i.e., incomplete) answer is that the garbage collector removes an object from the heap only if it is unreachable by any part of your code base. Assume you have a method in your Program class that allocates a local Car object as follows:

```
static void MakeACar()
{
    // If myCar is the only reference to the Car object, it *may* be destroyed when this method returns.
    Car myCar = new Car();
}
```

Notice that this Car reference (myCar) has been created directly within the MakeACar() method and has not been passed outside of the defining scope (via a return value or ref/out parameters). Thus, once this method call completes, the myCar reference is no longer reachable, and the associated Car object is now a candidate for garbage collection. Understand, however, that you can’t guarantee that this object will be reclaimed from memory immediately after MakeACar() has completed. All you can assume at this point is that when the runtime performs the next garbage collection, the myCar object could be safely destroyed.

As you will most certainly discover, programming in a garbage-collected environment greatly simplifies your application development. In stark contrast, C++ programmers are painfully aware that if they fail to manually delete heap-allocated objects, memory leaks are never far behind. In fact, tracking down memory leaks is one of the most time-consuming (and tedious) aspects of programming in unmanaged environments. By allowing the garbage collector to take charge of destroying objects, the burden of memory management has been lifted from your shoulders and placed onto those of the runtime.

# The CIL of new

When the C# compiler encounters the new keyword, it emits a CIL newobj instruction into the method implementation. If you compile the current example code and investigate the resulting assembly using ildasm.exe, you’d find the following CIL statements within the MakeACar() method:

```
.method assembly hidebysig static
    void '<Main>$g__MakeACar|0_0'() cil managed
{
    // Code size       8 (0x8)
    .maxstack 1
    .locals init (class SimpleGC.Car V_0)
    IL_0000: nop
    IL_0001: newobj     instance void SimpleGC.Car::.ctor()

```