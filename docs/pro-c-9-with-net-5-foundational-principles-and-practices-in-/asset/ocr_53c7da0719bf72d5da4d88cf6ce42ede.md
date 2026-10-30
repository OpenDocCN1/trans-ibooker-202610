```
.class public interface IMyOtherInterface
    implements MyNamespace.IMyInterface {}
```

# Defining Structures in CIL

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

# Defining Enums in CIL

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

# Defining Generics in CIL

Generic types also have a specific representation in the syntax of CIL. Recall from Chapter 10 that a given generic type or generic member may have one or more type parameters. For example, the List<T> type has a single type parameter, while Dictionary<TKey, TValue> has two. In terms of CIL, the number of type parameters is specified using a backward-leaning single tick (`), followed by a numerical value representing the number of type parameters. Like C#, the actual value of the type parameters is encased within angled brackets.

> Note On US keyboards, you can find the ` character on the key above the Tab key (and to the left of the 1 key).