While this syntax is quite helpful when you need to resolve name clashes, you can use explicit interface implementation simply to hide more “advanced” members from the object level. In this way, when the object user applies the dot operator, the user will see only a subset of the type’s overall functionality. However, those who require the more advanced behaviors can extract the desired interface via an explicit cast.

# Designing Interface Hierarchies

Interfaces can be arranged in an interface hierarchy. Like a class hierarchy, when an interface extends an existing interface, it inherits the abstract members defined by the parent (or parents). Prior to C# 8, derived interfaces never inherit true implementation. Rather, a derived interface simply extends its own definition with additional abstract members. In C# 8, derived interfaces inherit the default implementations as well as extend the definition and potentially add new default implementations.

Interface hierarchies can be useful when you want to extend the functionality of an existing interface without breaking existing code bases. To illustrate, create a new Console Application project named InterfaceHierarchy. Now, let’s design a new set of rendering-centric interfaces such that IDrawable is the root of the family tree.

```
namespace InterfaceHierarchy
{
    public interface IDrawable
    {
        void Draw();
    }
}
```

Given that IDrawable defines a basic drawing behavior, you could now create a derived interface that extends this interface with the ability to render in modified formats. Here is an example:

```
namespace InterfaceHierarchy
{
    public interface IAdvancedDraw : IDrawable
    {
        void DrawInBoundingBox(int top, int left, int bottom, int right);
        void DrawUpsideDown();
    }
}
```

Given this design, if a class were to implement IAdvancedDraw, it would now be required to implement every member defined up the chain of inheritance (specifically, the Draw(), DrawInBoundingBox(), and DrawUpsideDown() methods).

```
using System;
namespace InterfaceHierarchy
{
    public class BitmapImage : IAdvancedDraw
    {
        public void Draw()
        {
            Console.WriteLine("Drawing...");
        }
```