member that is no longer accessible or has been repositioned on the heap after surviving a generational sweep (which is an obvious problem).

To lock a reference type variable in memory from an unsafe context, C# provides the fixed keyword. The fixed statement sets a pointer to a managed type and “pins” that variable during the execution of the code. Without fixed, pointers to managed variables would be of little use, since garbage collection could relocate the variables unpredictably. (In fact, the C# compiler will not allow you to set a pointer to a managed variable except in a fixed statement.)

Thus, if you create a PointRef object and want to interact with its members, you must write the following code (or receive a compiler error):

```
unsafe static void UseAndPinPoint()
{
    PointRef pt = new PointRef
    {
        x = 5,
        y = 6
    };

    // Pin pt in place so it will not
    // be moved or GC-ed.
    fixed (int* p = &pt.x)
    {
        // Use int* variable here!
    }

    // pt is now unpinned, and ready to be GC-ed once
    // the method completes.
    Console.WriteLine ("Point is: {0}", pt);
}
```

In a nutshell, the fixed keyword allows you to build a statement that locks a reference variable in memory, such that its address remains constant for the duration of the statement (or scope block). Any time you interact with a reference type from within the context of unsafe code, pinning the reference is a must.

# The sizeof Keyword

The final unsafe-centric C# keyword to consider is sizeof. As in C++, the C# sizeof keyword is used to obtain the size in bytes of an intrinsic data type, but not a custom type, unless within an unsafe context. For example, the following method does not need to be declared “unsafe” as all arguments to the sizeof keyword are intrinsic types:

```
static void UseSizeOfOperator()
{
    Console.WriteLine("The size of short is {0}.", sizeof(short));
    Console.WriteLine("The size of int is {0}.", sizeof(int));
    Console.WriteLine("The size of long is {0}.", sizeof(long));
}
```