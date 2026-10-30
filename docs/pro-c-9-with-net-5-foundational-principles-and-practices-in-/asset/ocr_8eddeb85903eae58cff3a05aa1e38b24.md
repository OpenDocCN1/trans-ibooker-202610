that every shape needs to render itself in a unique manner, subclasses such as Hexagon and Circle are free to override this method to their own liking (see Figure 5-3).

![](bbox=[0.105, 0.155, 0.905, 0.405])

Figure 5-3. Classical polymorphism

After a polymorphic interface has been designed, you can begin to make various assumptions in your code. For example, given that Hexagon and Circle derive from a common parent (Shape), an array of Shape types could contain anything deriving from this base class. Furthermore, given that Shape defines a polymorphic interface to all derived types (the Draw() method in this example), you can assume each member in the array has this functionality.

Consider the following code, which instructs an array of Shape-derived types to render themselves using the Draw() method:

```
Shape[] myShapes = new Shape[3];
myShapes[0] = new Hexagon();
myShapes[1] = new Circle();
myShapes[2] = new Hexagon();

foreach (Shape s in myShapes)
{
    // Use the polymorphic interface!
    s.Draw();
}
Console.ReadLine();
```

This wraps up our brisk overview of the pillars of OOP. Now that you have the theory in your mind, the remainder of this chapter explores further details of how encapsulation is handled under C#, starting with a look at access modifiers. Chapter 6 will tackle the details of inheritance and polymorphism.