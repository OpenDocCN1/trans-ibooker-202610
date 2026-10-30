> Note This example is using top-level statements, which doesn't utilize a Main() method. Updating the Main() method to accept the args parameter is covered shortly.

Once again examining the generate IL for the program using top-level statements, notice that the <Main>$ method accepts a string array named args, as shown here in (abbreviated for space):

```
.class private abstract auto ansi sealed beforefieldinit '<Program>$'
    extends [System.Runtime]System.Object
{
    .custom instance void [System.Runtime]System.Runtime.CompilerServices.
CompilerGeneratedAttribute::.ctor()=
    ( 01 00 00 00 )
    .method private hidebysig static
        void '<Main>$' (string[] args) cil managed
    {
        .entrypoint
... } // end of method '<Program>$'::'<Main>$'
} // end of class '<Program>$'
```

If the program is still using a Main() method as the entry point, make sure the method signature accepts a string array named args, as follows:

```
static int Main(string[] args)
{
...
}
```

Here, you are checking to see whether the array of strings contains some number of items using the Length property of System.Array. As you will see in Chapter 4, all C# arrays actually alias the System.Array class and, therefore, share a common set of members. As you loop over each item in the array, its value is printed to the console window. Supplying the arguments at the command line is equally simple, as shown here:

```
C:\SimpleCSharpApp>dotnet run /arg1 -arg2

***** My First C# App *****
Hello World!
Arg: /arg1
Arg: -arg2
```

As an alternative to the standard for loop, you may iterate over an incoming string array using the C# foreach keyword. Here is some sample usage (but again, you will see specifics of looping constructs later in this chapter):

```
// Notice you have no need to check the size of the array when using "foreach".
// Process any incoming args using foreach.
foreach(string arg in args)
```