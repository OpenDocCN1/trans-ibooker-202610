
A better approach is to place each class in its own file, each with a namespace definition. To ensure each type is packaged into the same logical group, simply wrap the given class definitions in the same namespace scope, like so:

```
namespace MyShapes
{
    // Circle class
    public class Circle { /* Interesting methods... */ }
}
```

```
// Hexagon.cs
namespace MyShapes
{
    // Hexagon class
    public class Hexagon { /* More interesting methods... */ }
}
```

```
// Square.cs
namespace MyShapes
{
    // Square class
    public class Square { /* Even more interesting methods...*/}
}
```

Guidance It is considered a best practice to have only one class in each code file. While some of the earlier examples did not do this, that was for simplicity in teaching. Moving forward in this book, my intention is to always separate each class into its own file.

Notice how the MyShapes namespace acts as the conceptual “container” of these classes. When another namespace (such as CustomNamespaces) wants to use types in a separate namespace, you use the using keyword, just as you would when using namespaces of the .NET Core base class libraries, as follows:

```
// Bring in a namespace from the base class libraries.
using System;
// Make use of types defined the MyShapes namespace.
using MyShapes;

Hexagon h = new Hexagon();
Circle c = new Circle();
Square s = new Square();
```

For this example, the assumption is that the C# files that define the MyShapes namespace are part of the same Console Application project; in other words, all the files are compiled into a single assembly. If you defined the MyShapes namespace within an external assembly, you would also need to add a reference to that library before you could compile successfully. You’ll learn all the details of building applications that use external libraries during this chapter.

## Resolving Name Clashes with Fully Qualified Names

Technically speaking, you are not required to use the C# using keyword when referring to types defined in external namespaces. You could use the fully qualified name of the type, which, as you may recall from Chapter 1, is the type’s name prefixed with the defining namespace. Here’s an example:

```
// Note we are not importing MyShapes anymore!
using System;

MyShapes.Hexagon h = new MyShapes.Hexagon();
MyShapes.Circle c = new MyShapes.Circle();
MyShapes.Square s = new MyShapes.Square();
```

Typically, there is no need to use a fully qualified name. Not only does it require a greater number of keystrokes, it also makes no difference whatsoever in terms of code size or execution speed. In fact, in CIL code, types are always defined with the fully qualified name. In this light, the C# using keyword is simply a typing time-saver.

However, fully qualified names can be helpful (and sometimes necessary) to avoid potential name clashes when using multiple namespaces that contain identically named types. Assume you have a new namespace termed My3DShapes, which defines the following three classes, capable of rendering a shape in stunning 3D:

```
// Another shape-centric namespace.
//Circle.cs
namespace My3DShapes
{
    // 3D Circle class.
    public class Circle { }
}
//Hexagon.cs
namespace My3DShapes
{
    // 3D Hexagon class.
    public class Hexagon { }
}
//Square.cs
namespace My3DShapes
{
    // 3D Square class.
    public class Square { }
}
```

If you update the top-level statements as shown next, you are issued several compile-time errors, because both namespaces define identically named classes:

```
// Ambiguities abound!
using System;
using MyShapes;
using My3DShapes;

// Which namespace do I reference?
Hexagon h = new Hexagon(); // Compiler error!
```

```
Circle c = new Circle();  // Compiler error!
Square s = new Square();  // Compiler error!
```

The ambiguity can be resolved using the type’s fully qualified name, like so:

```
// We have now resolved the ambiguity.
My3DShapes.Hexagon h = new My3DShapes.Hexagon();
My3DShapes.Circle c = new My3DShapes.Circle();
MyShapes.Square s = new MyShapes.Square();
```

## Resolving Name Clashes with Aliases

The C# using keyword also lets you create an alias for a type’s fully qualified name. When you do so, you define a token that is substituted for the type’s full name at compile time. Defining aliases provides a second way to resolve name clashes. Here’s an example:

```
using System;
using MyShapes;
using My3DShapes;
```

```
// Resolve the ambiguity using a custom alias.
using The3DHexagon = My3DShapes.Hexagon;
```

```
// This is really creating a My3DShapes.Hexagon class.
The3DHexagon h2 = new The3DHexagon();
...
```

This alternative using syntax also lets you create an alias for a lengthy namespace. One of the longer base class library namespaces is System.Runtime.Serialization.Formatters.Binary, which contains a member named BinaryFormatter. If you want, you can create an instance of the BinaryFormatter as follows:

```
using bfHome = System.Runtime.Serialization.Formatters.Binary;
bfHome.BinaryFormatter b = new bfHome.BinaryFormatter();
...
```

as well as with a traditional using directive:

```
using System.Runtime.Serialization.Formatters.Binary;
BinaryFormatter b = new BinaryFormatter();
...
```

At this point in the game, there is no need to concern yourself with what the BinaryFormatter class is used for (you’ll examine this class in Chapter 20). For now, simply remember that the C# using keyword can be used to define aliases for lengthy fully qualified names or, more commonly, to resolve name clashes that can arise when importing multiple namespaces that define identically named types.

> Note  Be aware that overuse of C# aliases can result in a confusing code base. If other programmers on your team are unaware of your custom aliases, they could assume the aliases refer to types in the base class libraries and become quite confused when they can’t find these tokens in the documentation!

## Creating Nested Namespaces

When organizing your types, you are free to define namespaces within other namespaces. The base class libraries do so in numerous places to provide deeper levels of type organization. For example, the IO namespace is nested within System to yield System.IO.

The .NET Core project templates add the initial code in Program.cs into a namespace named after the project. This base namespace is referred to as the root namespace. In this example, the root namespace created by the .NET Core template is CustomNamespaces and is shown here:

```
namespace CustomNamespaces
{
    class Program
    {
    ...
    }
}
```

> Note When replacing the Program/Main() combination with top-level statements, you cannot assign a namespace to those top-level statements.

To nest the MyShapes and My3DShape namespaces inside the root namespace, there are two options. The first is to simply nest the namespace keyword, like this:

```
namespace CustomNamespaces
{
    namespace MyShapes
    {
        // Circle class
        public class Circle
        {
            /* Interesting methods... */
        }
    }
}
```

The other option (and more common) is to use “dot notation” in the namespace definition, like this:

```
namespace CustomNamespaces.MyShapes
{
    // Circle class
    public class Circle
    {
        /* Interesting methods... */
    }
}
```

Namespaces do not have to directly contain any types. This allows developers to use namespaces to provide a further level of scope.

Given that you have now nested the My3DShape namespace within the CustomNamespaces root namespace, you need to update any existing using directives and type aliases, like so (assuming you have updated all the sample classes to be nested under the root namespace):

```
using The3DHexagon = CustomNamespaces.My3DShape.Hexagon;
using CustomNamespaces.MyShapes;
```

Guidance A common practice is to group files in a namespace by directory. As shown, where a file lives in the directory structure has no impact on namespaces. However, it does make the namespace structure clearer (and concrete) to other developers. Therefore, many developers and code linting tools expect the namespaces to match the folder structures.

## Change the Root Namespace of Visual Studio

As mentioned, when you create a new C# project using Visual Studio (or the .NET Core CLI), the name of your application’s root namespace will be identical to the project name. From this point on, when you use Visual Studio to insert new code files using the Project ► Add New Item menu selection, types will automatically be wrapped within the root namespace. If you want to change the name of the root namespace, simply access the “Default namespace” option using the Application tab of the project’s properties window (see Figure 16-1).

![](img/587021f4d5ba039be70a11107130377b_643_0.png)

Figure 16-1. Configuring the default namespace
