# Table 19-8. Various Methods of ILGenerator

| Method | Meaning in Life |
|---|---|
| BeginCatchBlock() | Begins a catch block |
| BeginExceptionBlock() | Begins an exception scope for an exception |
| BeginFinallyBlock() | Begins a finally block |
| BeginScope() | Begins a lexical scope |
| DeclareLocal() | Declares a local variable |
| DefineLabel() | Declares a new label |
| Emit() | Is overloaded numerous times to allow you to emit CIL opcodes |
| EmitCall() | Pushes a call or callvirt opcode into the CIL stream |
| EmitWriteLine() | Emits a call to Console.WriteLine() with different types of values |
| EndExceptionBlock() | Ends an exception block |
| EndScope() | Ends a lexical scope |
| ThrowException() | Emits an instruction to throw an exception |
| UsingNamespace() | Specifies the namespace to be used in evaluating locals and watches for the current active lexical scope |

The key method of ILGenerator is Emit(), which works in conjunction with the System.Reflection.Emit.OpCodes class type. As mentioned earlier in this chapter, this type exposes a good number of read-only fields that map to raw CIL opcodes. The full set of these members are all documented within online help, and you will see various examples in the pages that follow.

# Emitting a Dynamic Assembly

To illustrate the process of defining a .NET Core assembly at runtime, let’s walk through the process of creating a single-file dynamic assembly. Within this assembly is a class named HelloWorld. The HelloWorld class supports a default constructor and a custom constructor that is used to assign the value of a private member variable (theMessage) of type string. In addition, HelloWorld supports a public instance method named SayHello(), which prints a greeting to the standard I/O stream, and another instance method named GetMsg(), which returns the internal private string. In effect, you are going to programmatically generate the following class type:

```
// This class will be created at runtime
// using System.Reflection.Emit.
public class HelloWorld
{
    private string theMessage;
    HelloWorld() {}
    HelloWorld(string s) {theMessage = s;}

    public string GetMsg() {return theMessage;}
    public void SayHello()
    {
        System.Console.WriteLine("Hello from the HelloWorld class!");
    }
}
```