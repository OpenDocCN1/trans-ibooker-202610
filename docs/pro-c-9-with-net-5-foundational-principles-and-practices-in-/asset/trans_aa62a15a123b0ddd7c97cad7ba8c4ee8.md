

# 表 19-8。ILGenerator 的各种方法

| 方法 | 实际含义 |
|---|---|
| BeginCatchBlock() | 开始一个 catch 块 |
| BeginExceptionBlock() | 开始一个异常的异常作用域 |
| BeginFinallyBlock() | 开始一个 finally 块 |
| BeginScope() | 开始一个词法作用域 |
| DeclareLocal() | 声明一个局部变量 |
| DefineLabel() | 声明一个新标签 |
| Emit() | 经过多次重载，可用于发出 CIL 操作码 |
| EmitCall() | 将 call 或 callvirt 操作码推入 CIL 流 |
| EmitWriteLine() | 发出对 Console.WriteLine() 的调用，并支持不同类型的值 |
| EndExceptionBlock() | 结束一个异常块 |
| EndScope() | 结束一个词法作用域 |
| ThrowException() | 发出用于引发异常的指令 |
| UsingNamespace() | 指定在当前活动词法作用域中计算局部变量和监视表达式时要使用的命名空间 |

ILGenerator 的关键方法是 Emit()，它与 System.Reflection.Emit.OpCodes 类类型配合使用。如本章前面所述，该类型公开了大量只读字段，这些字段映射到原始 CIL 操作码。这些成员的完整集合都记录在联机帮助中，后续页面中也会看到各种示例。

## 发出动态程序集

为了演示如何在运行时定义 .NET Core 程序集，下面介绍创建单文件动态程序集的过程。该程序集中包含一个名为 HelloWorld 的类。HelloWorld 类支持默认构造函数，以及一个用于为 string 类型私有成员变量（theMessage）赋值的自定义构造函数。此外，HelloWorld 还支持一个名为 SayHello() 的公共实例方法，该方法会向标准 I/O 流打印问候语；另一个实例方法 GetMsg() 则返回内部的私有字符串。实际上，你将以编程方式生成以下类类型：

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

假设你已经创建了一个名为 DynamicAsmBuilder 的新控制台应用程序项目，并添加了 System.Reflection.Emit NuGet 包。接下来，导入 System.Reflection 和 System.Reflection.Emit 命名空间。在 Program 类中定义一个名为 CreateMyAsm() 的静态方法。这个方法负责完成以下工作：

- 定义动态程序集的特征（名称、版本等）
- 实现 HelloClass 类型
- 将 AssemblyBuilder 返回给调用方法

下面是完整代码，随后将进行分析：

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

```
constructorIl.Emit(OpCodes.Ldarg_0);
constructorIl.Emit(OpCodes.Ldarg_1);
constructorIl.Emit(OpCodes.Stfld, msgField);
constructorIl.Emit(OpCodes.Ret);

// Create the default ctor.
helloWorldClass.DefineDefaultConstructor(
    MethodAttributes.Public);
// Now create the GetMsg() method.
MethodBuilder getMsgMethod = helloWorldClass.DefineMethod(
    "GetMsg",
    MethodAttributes.Public,
    typeof(string),
    null);
ILGenerator methodIl = getMsgMethod.GetILGenerator();
methodIl.Emit(OpCodes.Ldarg_0);
methodIl.Emit(OpCodes.Ldfld, msgField);
methodIl.Emit(OpCodes.Ret);

// Create the SayHello method.
MethodBuilder sayHiMethod = helloWorldClass.DefineMethod(
    "SayHello", MethodAttributes.Public, null, null);
methodIl = sayHiMethod.GetILGenerator();
methodIl.EmitWriteLine("Hello from the HelloWorld class!");
methodIl.Emit(OpCodes.Ret);

// "Bake" the class HelloWorld.
// (Baking is the formal term for emitting the type.)
helloWorldClass.CreateType();

return builder;
}
```

## 发出程序集和模块集合

方法体首先使用 AssemblyName 和 Version 类型（定义在 System.Reflection 命名空间中）建立程序集的最小特征集合。接下来，通过静态 AssemblyBuilder.DefineDynamicAssembly() 方法获取 AssemblyBuilder 类型。

调用 DefineDynamicAssembly() 时，必须指定要定义的程序集的访问模式。最常见的值如表 19-9 所示。

表 19-9。AssemblyBuilderAccess 枚举的常见值

| 值 | 实际含义 |
|---|---|
| RunAndCollect | 程序集会立即被卸载，并在其不再可访问后回收所占用的内存。 |
| Run | 表示动态程序集可以在内存中执行，但不能保存到磁盘。 |

下一项任务是为新程序集定义模块集合（及其名称）。DefineDynamicModule() 方法返回后，你将获得一个指向有效 ModuleBuilder 类型的引用。

```
// Create new assembly.
var builder = AssemblyBuilder.DefineDynamicAssembly(
    assemblyName, AssemblyBuilderAccess.Run);
```

## ModuleBuilder 类型的作用

ModuleBuilder 是开发动态程序集时使用的关键类型。不出所料，ModuleBuilder 支持多个成员，可以定义给定模块中包含的类型集合（类、接口、结构等），以及其中包含的嵌入资源集合（字符串表、图像等）。表 19-10 描述了其中两个以创建为中心的方法。（请注意，每个方法都会返回一个相关类型，用来表示要构造的类型。）

# 表 19-10。ModuleBuilder 类型的部分成员

| 方法 | 实际含义 |
|---|---|
| DefineEnum() | 用于发出 .NET 枚举定义 |
| DefineType() | 构造一个 TypeBuilder，使你能够定义值类型、接口和类类型（包括委托） |

需要了解的 ModuleBuilder 类的关键成员是 DefineType()。除了通过简单字符串指定类型名称外，还需要使用 System.Reflection.TypeAttributes 枚举描述类型本身的格式。表 19-11 列出了 TypeAttributes 枚举的一些（但不是全部）关键成员。

# 表 19-11。TypeAttributes 枚举的部分成员

| 成员 | 实际含义 |
|---|---|
| Abstract | 指定该类型为抽象类型 |
| Class | 指定该类型为类 |
| Interface | 指定该类型为接口 |
| NestedAssembly | 指定该类为嵌套类，具有程序集可见性，因此只有其程序集中的方法才能访问它 |
| NestedFamANDAssem | 指定该类为嵌套类，同时具有程序集和族可见性，因此只有处于其族和程序集交集中的方法才能访问它 |
| NestedFamily | 指定该类为嵌套类，具有族可见性，因此只有其自身类型及其任何子类型中的方法才能访问它 |
| NestedFamORAssem | 指定该类为嵌套类，具有族或程序集可见性，因此只有处于其族和程序集并集中的方法才能访问它 |
| NestedPrivate | 指定该类为嵌套类，具有私有可见性 |
| NestedPublic | 指定该类为嵌套类，具有公共可见性 |
| NotPublic | 指定该类不是公共类 |
| Public | 指定该类为公共类 |
| Sealed | 指定该类为具体类，不能被扩展 |
| Serializable | 指定该类可以序列化 |

