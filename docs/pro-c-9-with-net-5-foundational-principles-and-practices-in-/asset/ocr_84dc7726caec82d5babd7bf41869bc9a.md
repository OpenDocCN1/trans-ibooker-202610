```
public static void MyLocalVariables()
{
    string myStr = "CIL code is fun!";
    int myInt = 33;
    object myObj = new object();
}
```

If you were to construct MyLocalVariables() directly in CIL, you could author the following:

```
.method public hidebysig static void
    MyLocalVariables() cil managed
{
    .maxstack 8
    // Define three local variables.
    .locals init (string myStr, int32 myInt, object myObj)
    // Load a string onto the virtual execution stack.
    ldstr "CIL code is fun!"
    // Pop off current value and store in local variable [0].
    stloc.0

    // Load a constant of type "i4"
    // (shorthand for int32) set to the value 33.
    ldc.i4.s 33
    // Pop off current value and store in local variable [1].
    stloc.1

    // Create a new object and place on stack.
    newobj instance void [mscorlib]System.Object::.ctor()
    // Pop off current value and store in local variable [2].
    stloc.2
    ret
}
```

The first step taken to allocate local variables in raw CIL is to use the .locals directive, which is paired with the init attribute. Each variable is identified by its data type and an optional variable name. After the local variables have been defined, you load a value onto the stack (using the various load-centric opcodes) and store the value within the local variable (using the various storage-centric opcodes).

# Mapping Parameters to Local Variables in CIL

You have already seen how to declare local variables in raw CIL using the .locals init directive; however, you have yet to see exactly how to map incoming parameters to local methods. Consider the following static C# method:

```
public static int Add(int a, int b)
{
    return a + b;
}
```