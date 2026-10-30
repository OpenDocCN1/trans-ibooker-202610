Here are the results:

```
***** Simple Interface Hierarchy *****
...
***** Calling Implemented TimeToDraw *****
Time to draw: 12
Time to draw: 12
Time to draw: 12
```

# Multiple Inheritance with Interface Types

Unlike class types, an interface can extend multiple base interfaces, allowing you to design some powerful and flexible abstractions. Create a new Console Application project named MiInterfaceHierarchy. Here is another collection of interfaces that model various rendering and shape abstractions. Notice that the IShape interface is extending both IDrawable and IPrintable.

```
//IDrawable.cs
namespace MiInterfaceHierarchy
{
    // Multiple inheritance for interface types is A-okay.
    interface IDrawable
    {
        void Draw();
    }
}
```

```
//IPrintable.cs
namespace MiInterfaceHierarchy
{
    interface IPrintable
    {
        void Print();
        void Draw(); // <-- Note possible name clash here!
    }
}
```

```
//IShape.cs
namespace MiInterfaceHierarchy
{
    // Multiple interface inheritance. OK!
    interface IShape : IDrawable, IPrintable
    {
        int GetNumberOfSides();
    }
}
```