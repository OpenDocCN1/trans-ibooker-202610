
## Emitting the HelloClass Type and the String Member Variable

Now that you have a better understanding of the role of the ModuleBuilder.CreateType() method, let’s examine how you can emit the public HelloWorld class type and the private string variable.

// Define a public class named "HelloWorld".
TypeBuilder helloWorldClass =
    module.DefineType("MyAssembly.HelloWorld",
    TypeAttributes.Public);

// Define a private String variable named "theMessage".
FieldBuilder msgField = helloWorldClass.DefineField(
    "theMessage",
    Type.GetType("System.String"),
    attributes: FieldAttributes.Private);

Notice how the TypeBuilder.DefineField() method provides access to a FieldBuilder type. The TypeBuilder class also defines other methods that provide access to other "builder" types. For example, DefineConstructor() returns a ConstructorBuilder, DefineProperty() returns a PropertyBuilder, and so forth.

## Emitting the Constructors

As mentioned earlier, the TypeBuilder.DefineConstructor() method can be used to define a constructor for the current type. However, when it comes to implementing the constructor of HelloClass, you need to inject raw CIL code into the constructor body, which is responsible for assigning the incoming parameter to the internal private string. To obtain an ILGenerator type, you call the GetILGenerator() method from the respective "builder" type you have reference to (in this case, the ConstructorBuilder type).

The Emit() method of the ILGenerator class is the entity in charge of placing CIL into a member implementation. Emit() itself makes frequent use of the OpCodes class type, which exposes the opcode set of CIL using read-only fields. For example, OpCodes.Ret signals the return of a method call, OpCodes.Stfld makes an assignment to a member variable, and OpCodes.Call is used to call a given method (in this case, the base class constructor). That said, ponder the following constructor logic:

// Create the custom ctor taking single string arg.
Type[] constructorArgs = new Type[1];
constructorArgs[0] = typeof(string);
ConstructorBuilder constructor =
    helloWorldClass.DefineConstructor(
        MethodAttributes.Public,
        CallingConventions.Standard,
        constructorArgs);
//Emit the necessary CIL into the ctor
ILGenerator constructorIl = constructor.GetILGenerator();
constructorIl.Emit(OpCodes.Ldarg_0);
Type objectClass = typeof(object);
ConstructorInfo superConstructor =
    objectClass.GetConstructor(new Type[0]);
constructorIl.Emit(OpCodes.Call, superConstructor);
//Load this pointer onto the stack
constructorIl.Emit(OpCodes.Ldarg_0);
constructorIl.Emit(OpCodes.Ldarg_1);
//Load argument on virtual stack and store in msgField
constructorIl.Emit(OpCodes.Stfld, msgField);
constructorIl.Emit(OpCodes.Ret);

Now, as you are aware, as soon as you define a custom constructor for a type, the default constructor is silently removed. To redefine the no-argument constructor, simply call the DefineDefaultConstructor() method of the TypeBuilder type as follows:

// Create the default ctor.
helloWorldClass.DefineDefaultConstructor(
    MethodAttributes.Public);

## Emitting the SayHello() Method

Finally, let's examine the process of emitting the SayHello() method. The first task is to obtain a MethodBuilder type from the helloWorldClass variable. After you do this, you define the method and obtain the underlying ILGenerator to inject the CIL instructions, like so:

// Create the SayHello method.
MethodBuilder sayHiMethod = helloWorldClass.DefineMethod(
    "SayHello", MethodAttributes.Public, null, null);
methodIL = sayHiMethod.GetILGenerator();

//Write to the console
methodIL.EmitWriteLine("Hello from the HelloWorld class!");
methodIL.Emit(OpCodes.Ret);

Here you have established a public method (MethodAttributes.Public) that takes no parameters and returns nothing (marked by the null entries contained in the DefineMethod() call). Also note the EmitWriteLine() call. This helper member of the ILGenerator class automatically writes a line to the standard output with minimal fuss and bother.

## Using the Dynamically Generated Assembly

Now that you have the logic in place to create your assembly, all that is needed is to execute the generated code. The logic in the calling code calls the CreateMyAsm() method, getting a reference to the created AssemblyBuilder.

Next, you will exercise some late binding (see Chapter 17) to create an instance of the HelloWorld class and interact with its members. Update your top-level statements as follows:

using System;
using System.Reflection;
using System.Reflection.Emit;

Console.WriteLine("***** The Amazing Dynamic Assembly Builder App *****");
// Create the assembly builder using our helper f(x).
AssemblyBuilder builder = CreateMyAsm();

// Get the HelloWorld type.
Type hello = builder.GetType("MyAssembly.HelloWorld");

// Create HelloWorld instance and call the correct ctor.
Console.Write("-> Enter message to pass HelloWorld class: ");
string msg = Console.ReadLine();
object[] ctorArgs = new object[1];
ctorArgs[0] = msg;
object obj = Activator.CreateInstance(hello, ctorArgs);

// Call SayHello and show returned string.
Console.WriteLine("-> Calling SayHello() via late binding.");
MethodInfo mi = hello.GetMethod("SayHello");
mi.Invoke(obj, null);

// Invoke method.
mi = hello.GetMethod("GetMsg");
Console.WriteLine(mi.Invoke(obj, null));

In effect, you have just created a .NET Core assembly that is able to create and execute .NET Core assemblies at runtime! That wraps up the examination of CIL and the role of dynamic assemblies. I hope this chapter has deepened your understanding of the .NET Core type system, the syntax and semantics of CIL, and how the C# compiler processes your code at compile time.

## Summary

This chapter provided an overview of the syntax and semantics of CIL. Unlike higher-level managed languages such as C#, CIL does not simply define a set of keywords but provides directives (used to define the structure of an assembly and its types), attributes (which further qualify a given directive), and opcodes (which are used to implement type members).

You were introduced to a few CIL-centric programming tools and learned how to alter the contents of a .NET assembly with new CIL instructions using round-trip engineering. After this point, you spent time learning how to establish the current (and referenced) assembly, namespaces, types, and members. I wrapped up with a simple example of building a .NET code library and executable using little more than CIL, command-line tools, and a bit of elbow grease.

Finally, you took an introductory look at the process of creating a dynamic assembly. Using the System.Reflection.Emit namespace, it is possible to define a .NET Core assembly in memory at runtime. As you have seen firsthand, using this API requires you to know the semantics of CIL code in some detail. While the need to build dynamic assemblies is certainly not a common task for most .NET Core applications, it can be useful for those of you who need to build support tools and other programming utilities.

## PART VI

## File Handling, Object Serialization, and Data Access

## CHAPTER 20

## File I/O and Object Serialization

When you create desktop applications, the ability to save information between user sessions is commonplace. This chapter examines several I/O-related topics as seen through the eyes of the .NET Core Framework. The first order of business is to explore the core types defined in the System.IO namespace and learn how to modify a machine's directory and file structure programmatically. The next task is to explore various ways to read from and write to character-based, binary-based, string-based, and memory-based data stores.

After you learn how to manipulate files and directories using the core I/O types, you will examine the related topic of object serialization. You can use object serialization to persist and retrieve the state of an object to (or from) any System.IO.Stream-derived type.

> Note To ensure you can run each of the examples in this chapter, start Visual Studio with administrative rights (just right-click the Visual Studio icon and select Run as Administrator). If you do not do so, you may encounter runtime security exceptions when accessing the computer file system.

## Exploring the System.IO Namespace

In the framework of .NET Core, the System.IO namespace is the region of the base class libraries devoted to file-based (and memory-based) input and output (I/O) services. Like any namespace, System.IO defines a set of classes, interfaces, enumerations, structures, and delegates, most of which you can find in mscorlib.dll. In addition to the types contained within mscorlib.dll, the System.dll assembly defines additional members of the System.IO namespace.

Many of the types within the System.IO namespace focus on the programmatic manipulation of physical directories and files. However, additional types provide support to read data from and write data to string buffers, as well as raw memory locations. Table 20-1 outlines the core (nonabstract) classes, providing a road map of the functionality in System.IO.
