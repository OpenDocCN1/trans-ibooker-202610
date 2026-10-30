```
finally
{
    // Always call Dispose(), error or not.
    rw.Dispose();
}
```

While this is a fine example of defensive programming, the truth of the matter is that few developers are thrilled by the prospects of wrapping every disposable type within a try/finally block just to ensure the Dispose() method is called. To achieve the same result in a much less obtrusive manner, C# supports a special bit of syntax that looks like this:

```
Console.WriteLine("**** Fun with Dispose ****\n");
// Dispose() is called automatically when the using scope exits.
using(MyResourceWrapper rw = new MyResourceWrapper())
{
    // Use rw object.
}
```

If you looked at the following CIL code of the top-level statements using ildasm.exe, you would find the using syntax does indeed expand to try/finally logic, with the expected call to Dispose():

```
.method private hidebysig static void
    '<Main>$' (string[] args) cil managed
{
...
    .try
    {
    } // end .try
    finally
    {
        IL_0019:  callvirt   instance void [System.Runtime]System.IDisposable::Dispose()
    } // end handler
} // end of method '<Program>$'::'<Main>$'
```

> Note  If you attempt to “use” an object that does not implement IDisposable, you will receive a compiler error.

While this syntax does remove the need to manually wrap disposable objects within try/finally logic, the C# using keyword unfortunately now has a double meaning (importing namespaces and invoking a Dispose() method). Nevertheless, when you are working with types that support the IDisposable interface, this syntactical construct will ensure that the object “being used” will automatically have its Dispose() method called once the using block has exited.

Also, be aware that it is possible to declare multiple objects of the same type within a using scope. As you would expect, the compiler will inject code to call Dispose() on each declared object.

```
// Use a comma-delimited list to declare multiple objects to dispose.
using(MyResourceWrapper rw = new MyResourceWrapper(), rw2 = new MyResourceWrapper())
{
    // Use rw and rw2 objects.
}
```