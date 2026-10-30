# Recall: Conversions Among Related Class Types

As shown in Chapter 6, class types may be related by classical inheritance (the “is-a” relationship). In this case, the C# conversion process allows you to cast up and down the class hierarchy. For example, a derived class can always be implicitly cast to a base type. However, if you want to store a base class type in a derived variable, you must perform an explicit cast, like so:

```
// Two related class types.
class Base{}
class Derived : Base{}

// Implicit cast between derived to base.
Base myBaseType;
myBaseType = new Derived();
// Must explicitly cast to store base reference
// in derived type.
Derived myDerivedType = (Derived)myBaseType;
```

This explicit cast works because the Base and Derived classes are related by classical inheritance and myBaseType is constructed as an instance of Derived. However, if myBaseType is an instance of Base, the cast throws an InvalidCastException. If there is any doubt that the cast will fail, you should use the as keyword, as discussed in Chapter 6. Here is the sample reworked to demonstrate this:

```
// Implicit cast between derived to base.
Base myBaseType2 = new();
// Throws InvalidCastException
//Derived myDerivedType2 = (Derived)myBaseType2 as Derived;
//No exception, myDerivedType2 is null
Derived myDerivedType2 = myBaseType2 as Derived;
```

However, what if you have two class types in different hierarchies with no common parent (other than System.Object) that require conversions? Given that they are not related by classical inheritance, typical casting operations offer no help (and you would get a compiler error to boot!)

On a related note, consider value types (structures). Assume you have two structures named Square and Rectangle. Given that structures cannot leverage classic inheritance (as they are always sealed), you have no natural way to cast between these seemingly related types.

While you could create helper methods in the structures (such as Rectangle.ToSquare()), C# lets you build custom conversion routines that allow your types to respond to the () casting operator. Therefore, if you configured the structures correctly, you would be able to use the following syntax to explicitly convert between them as follows:

```
// Convert a Rectangle to a Square!
Rectangle rect = new Rectangle
{
    Width = 3;
    Height = 10;
}
Square sq = (Square)rect;
```