Assume you have created a new Console Application project named DynamicAsmBuilder and you add the System.Reflection.Emit NuGet package. Next, import the System.Reflection and System.Reflection.Emit namespaces. Define a static method named CreateMyAsm() in the Program class. This single method oversees the following:

+   - Defining the characteristics of the dynamic assembly (name, version, etc.)
+   - Implementing the HelloClass type
+   - Returning the AssemblyBuilder to the calling method

Here is the complete code, with analysis to follow:

```
static AssemblyBuilder CreateMyAsm()
{
    // Establish general assembly characteristics.
    AssemblyName assemblyName = new AssemblyName
    {
        Name = "MyAssembly",
        Version = new Version("1.0.0.0")
    };

    // Create new assembly.
    var builder = AssemblyBuilder.DefineDynamicAssembly(
        assemblyName, AssemblyBuilderAccess.Run);

    // Define the name of the module.
    ModuleBuilder module =
        builder.DefineDynamicModule("MyAssembly");
    // Define a public class named "HelloWorld".
    TypeBuilder helloWorldClass =
        module.DefineType("MyAssembly.HelloWorld",
        TypeAttributes.Public);

    // Define a private String variable named "theMessage".
    FieldBuilder msgField = helloWorldClass.DefineField(
        "theMessage",
        Type.GetType("System.String"),
        attributes: FieldAttributes.Private);

    // Create the custom ctor.
    Type[] constructorArgs = new Type[1];
    constructorArgs[0] = typeof(string);
    ConstructorBuilder constructor =
        helloWorldClass.DefineConstructor(
            MethodAttributes.Public,
            CallingConventions.Standard,
            constructorArgs);
    ILGenerator constructorIl = constructor.GetILGenerator();
    constructorIl.Emit(OpCodes.Ldarg_0);
    Type objectClass = typeof(object);
    ConstructorInfo superConstructor =
        objectClass.GetConstructor(new Type[0]);
    constructorIl.Emit(OpCodes.Call, superConstructor);
```