
Like C#, CIL allows you to define a nested namespace as follows:

```
// Defining a nested namespace.
.namespace MyCompany.MyNamespace {}
```

## Defining Class Types in CIL

Empty namespaces are not remarkably interesting, so let’s now check out the process of defining a class type using CIL. Not surprisingly, the .class directive is used to define a new class. However, this simple directive can be adorned with numerous additional attributes, to further qualify the nature of the type. To illustrate, add a public class to your namespace named MyBaseClass. As in C#, if you do not specify an explicit base class, your type will automatically be derived from System.Object.

```
.namespace MyNamespace
{
    // System.Object base class assumed.
    .class public MyBaseClass {}
}
```

When you are building a class type that derives from any class other than System.Object, you use the extends attribute. Whenever you need to reference a type defined within the same assembly, CIL demands that you also use the fully qualified name (however, if the base type is within the same assembly, you can omit the assembly’s friendly name prefix). Therefore, the following attempt to extend MyBaseClass results in a compiler error:

```
// This will not compile!
.namespace MyNamespace
{
    .class public MyBaseClass {}

    .class public MyDerivedClass
        extends MyBaseClass {}
}
```

To correctly define the parent class of MyDerivedClass, you must specify the full name of MyBaseClass as follows:

```
// Better!
.namespace MyNamespace
{
    .class public MyBaseClass {}

    .class public MyDerivedClass
        extends MyNamespace.MyBaseClass {}
}
```

In addition to the public and extends attributes, a CIL class definition may take numerous additional qualifiers that control the type’s visibility, field layout, and so on. Table 19-2 illustrates some (but not all) of the attributes that may be used in conjunction with the .class directive.

**Table 19-2. Various Attributes Used in Conjunction with the .class Directive**

| Attributes | Meaning in Life |
|---|---|
| public, private, nested assembly, nested famandassem, nested family, nested famorassem, nested public, nested private | CIL defines various attributes that are used to specify the visibility of a given type. As you can see, raw CIL offers numerous possibilities other than those offered by C#. Refer to ECMA 335 for details if you are interested. |
| abstract, sealed | These two attributes may be tacked onto a .class directive to define an abstract class or sealed class, respectively. |
| auto, sequential, explicit | These attributes are used to instruct the CLR how to lay out field data in memory. For class types, the default layout flag (auto) is appropriate. Changing this default can be helpful if you need to use P/Invoke to call into unmanaged C code. |
| extends, implements | These attributes allow you to define the base class of a type (via extends) or implement an interface on a type (via implements). |

## Defining and Implementing Interfaces in CIL

As odd as it might seem, interface types are defined in CIL using the .class directive. However, when the .class directive is adorned with the interface attribute, the type is realized as a CTS interface type. Once an interface has been defined, it may be bound to a class or structure type using the CIL implements attribute, like so:

```
.namespace MyNamespace
{
    // An interface definition.
    .class public interface IMyInterface {}

    // A simple base class.
    .class public MyBaseClass {}

    // MyDerivedClass now implements IMyInterface,
    // and extends MyBaseClass.
    .class public MyDerivedClass
        extends MyNamespace.MyBaseClass
        implements MyNamespace.IMyInterface {}
}
```

> Note The extends clause must precede the implements clause. As well, the implements clause can incorporate a comma-separated list of interfaces.

As you recall from Chapter 10, interfaces can function as the base interface to other interface types to build interface hierarchies. However, contrary to what you might be thinking, the extends attribute cannot be used to derive interface A from interface B. The extends attribute is used only to qualify a type's base class. When you want to extend an interface, you will use the implements attribute yet again. Here is an example:

```
// Extending interfaces in terms of CIL.
.class public interface IMyInterface {}
.class public interface IMyOtherInterface
    implements MyNamespace.IMyInterface {}
```

## Defining Structures in CIL

The .class directive can be used to define a CTS structure if the type extends System.ValueType. As well, the .class directive must be qualified with the sealed attribute (given that structures can never be a base structure to other value types). If you attempt to do otherwise, ilasm.exe will issue a compiler error.

```
// A structure definition is always sealed.
.class public sealed MyStruct
    extends [System.Runtime]System.ValueType{}
```

Do be aware that CIL provides a shorthand notation to define a structure type. If you use the value attribute, the new type will derive the type from [System.Runtime]System.ValueType automatically. Therefore, you could define MyStruct as follows:

```
// Shorthand notation for declaring a structure.
.class public sealed value MyStruct{}
```

## Defining Enums in CIL

.NET Core enumerations (as you recall) derive from System.Enum, which is a System.ValueType (and therefore must also be sealed). When you want to define an enum in terms of CIL, simply extend [System.Runtime]System.Enum, like so:

```
// An enum.
.class public sealed MyEnum
    extends [System.Runtime]System.Enum{}
```

Like a structure definition, enumerations can be defined with a shorthand notation using the enum attribute. Here is an example:

```
// Enum shorthand.
.class public sealed enum MyEnum{}
```

You will see how to specify the name-value pairs of an enumeration in just a moment.

## Defining Generics in CIL

Generic types also have a specific representation in the syntax of CIL. Recall from Chapter 10 that a given generic type or generic member may have one or more type parameters. For example, the List<T> type has a single type parameter, while Dictionary<TKey, TValue> has two. In terms of CIL, the number of type parameters is specified using a backward-leaning single tick (\`), followed by a numerical value representing the number of type parameters. Like C#, the actual value of the type parameters is encased within angled brackets.

> Note On US keyboards, you can find the \` character on the key above the Tab key (and to the left of the 1 key).

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

## Compiling the CILTypes.il File

Even though you have not yet added any members or implementation code to the types you have defined, you are able to compile this *.il file into a .NET Core DLL assembly (which you must do, as you have not specified a Main() method). Open a command prompt and enter the following command:

```
dotnet build
```

After you have done so, you can now open your compiled assembly into ilasm.exe to verify the creation of each type. To understand how to populate a type with content, you first need to examine the fundamental data types of CIL.

## .NET Base Class Library, C#, and CIL Data Type Mappings

Table 19-3 illustrates how a .NET base class type maps to the corresponding C# keyword and how each C# keyword maps into raw CIL. As well, Table 19-3 documents the shorthand constant notations used for each CIL type. As you will see in just a moment, these constants are often referenced by numerous CIL opcodes.
