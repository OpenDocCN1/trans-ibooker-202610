```
// OK! Can access public members
// of a parent within a derived type.
Speed = 10;

// Error! Cannot access private
// members of parent within a derived type.
_currSpeed = 10;
}
}
```

# Regarding Multiple Base Classes

Speaking of base classes, it is important to keep in mind that C# demands that a given class have exactly one direct base class. It is not possible to create a class type that directly derives from two or more base classes (this technique, which is supported in unmanaged C++, is known as multiple inheritance, or simply MI). If you attempted to create a class that specifies two direct parent classes, as shown in the following code, you would receive compiler errors:

```
// Illegal! C# does not allow
// multiple inheritance for classes!
class WontWork
: BaseClassOne, BaseClassTwo
{}
```

As you will see in Chapter 8, the .NET Core platform does allow a given class, or structure, to implement any number of discrete interfaces. In this way, a C# type can exhibit a number of behaviors while avoiding the complexities associated with MI. Using this technique, you can build sophisticated interface hierarchies that model complex behaviors (again, see Chapter 8).

# Using the sealed Keyword

C# supplies another keyword, sealed, that prevents inheritance from occurring. When you mark a class as sealed, the compiler will not allow you to derive from this type. For example, assume you have decided that it makes no sense to further extend the MiniVan class.

```
// The MiniVan class cannot be extended!
sealed class MiniVan : Car
{
}
```

If you (or a teammate) were to attempt to derive from this class, you would receive a compile-time error.

```
// Error! Cannot extend
// a class marked with the sealed keyword!
class DeluxeMiniVan
: MiniVan
{}
```