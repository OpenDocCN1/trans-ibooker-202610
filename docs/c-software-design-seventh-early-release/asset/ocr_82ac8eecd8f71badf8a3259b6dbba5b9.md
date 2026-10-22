```
return EXIT_SUCCESS;
}
```

We first create shape1 as an abstraction for a Circle and an associated drawing Strategy. This feels easy, right? There is no need to manually allocate and no need to deal with pointers. By means of the draw() function we are able to draw this Shape ⑦. Directly afterwards we create a copy of the shape. A real copy, i.e. a “deep copy”, not just the copy of a pointer. Drawing the copy by means of the draw() function will result in the same output ⑧. Again, this feels good: You can rely on the copy operations of the value type (in this case the copy constructor) and you do not have to clone() manually.

Pretty amazing, right? And definitely much better than using External Polymorphism manually. I admit that after all these implementation details it may be a little hard to see it right away, but if you step through the jungle of implementation details, I hope that you realize the beauty of this approach: you no longer have to deal with pointers, there are no manual allocations, and you do not have to deal with inheritance hierarchies anymore. All of these details are there, yes, but all evidence of these are nicely encapsulated within the Shape class. Still, you did not loose any of the decoupling benefits: you are still able to easily add new types and the concrete shape types are still oblivious about the drawing behavior. They are only connected to the desired functionality via the Shape constructor.

“I am wondering”, you are starting to ask. “Couldn’t we make this much easier? I envision a main() function that looks like this”:

```
//---- <YourMain.cpp> ----------------

int main()
{
    // Create a circle as one representative of a concrete shape type
    Circle circle{ 3.14 };

    // Bind the circle to some drawing functionality
    auto drawingCircle = [=]() { myCircleDrawer(circle); };
```