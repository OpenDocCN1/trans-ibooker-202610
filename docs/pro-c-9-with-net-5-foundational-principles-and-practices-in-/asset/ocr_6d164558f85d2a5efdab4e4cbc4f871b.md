For example, assume you want to create a List<T> variable, where T is of type System.Int32. In C#, you would type the following:

```
void SomeMethod()
{
    List<int> myInts = new List<int>();
}
```

In CIL, you would author the following (which could appear in any CIL method scope):

```
// In C#: List<int> myInts = new List<int>();
newobj instance void class [System.Collections] System.Collections.Generic.List`1<int32>::.ctor()
```

Notice that this generic class is defined as List`1<int32>, as List<T> has a single type parameter. However, if you needed to define a Dictionary<string, int> type, you would do so as follows:

```
// In C#: Dictionary<string, int> d = new Dictionary<string, int>();
newobj instance void class [System.Collections] System.Collections.Generic.Dictionary`2<string,int32>::.ctor()
```

As another example, if you have a generic type that uses another generic type as a type parameter, you would author CIL code such as the following:

```
// In C#: List<List<int>> myInts = new List<List<int>>();
newobj instance void class [mscorlib] System.Collections.Generic.List`1<class [System.Collections] System.Collections.Generic.List`1<int32>>::.ctor()
```

# Compiling the CILTypes.il File

Even though you have not yet added any members or implementation code to the types you have defined, you are able to compile this *.il file into a .NET Core DLL assembly (which you must do, as you have not specified a Main() method). Open a command prompt and enter the following command:

```
dotnet build
```

After you have done so, you can now open your compiled assembly into ilasm.exe to verify the creation of each type. To understand how to populate a type with content, you first need to examine the fundamental data types of CIL.

# .NET Base Class Library, C#, and CIL Data Type Mappings

Table 19-3 illustrates how a .NET base class type maps to the corresponding C# keyword and how each C# keyword maps into raw CIL. As well, Table 19-3 documents the shorthand constant notations used for each CIL type. As you will see in just a moment, these constants are often referenced by numerous CIL opcodes.