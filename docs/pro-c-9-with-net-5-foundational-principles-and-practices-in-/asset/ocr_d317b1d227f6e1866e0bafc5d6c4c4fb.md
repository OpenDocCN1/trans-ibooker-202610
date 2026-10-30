Given that you have now nested the My3DShape namespace within the CustomNamespaces root namespace, you need to update any existing using directives and type aliases, like so (assuming you have updated all the sample classes to be nested under the root namespace):

```
using The3DHexagon = CustomNamespaces.My3DShape.Hexagon;
using CustomNamespaces.MyShapes;
```

Guidance A common practice is to group files in a namespace by directory. As shown, where a file lives in the directory structure has no impact on namespaces. However, it does make the namespace structure clearer (and concrete) to other developers. Therefore, many developers and code linting tools expect the namespaces to match the folder structures.

# Change the Root Namespace of Visual Studio

As mentioned, when you create a new C# project using Visual Studio (or the .NET Core CLI), the name of your application’s root namespace will be identical to the project name. From this point on, when you use Visual Studio to insert new code files using the Project ► Add New Item menu selection, types will automatically be wrapped within the root namespace. If you want to change the name of the root namespace, simply access the “Default namespace” option using the Application tab of the project’s properties window (see Figure 16-1).

![](bbox=[0.07, 0.485, 0.895, 0.845])

Figure 16-1. Configuring the default namespace