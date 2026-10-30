Consider the following examples:

```
// Init a standard array.
int[] myArrayOfInts = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

```
// Init a generic List<> of ints.
List<int> myGenericList = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

```
// Init an ArrayList with numerical data.
ArrayList myList = new ArrayList { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

If your container is managing a collection of classes or a structure, you can blend object initialization syntax with collection initialization syntax to yield some functional code. You might recall the Point class from Chapter 5, which defined two properties named X and Y. If you wanted to build a generic List<T> of Point objects, you could write the following:

```
List<Point> myListOfPoints = new List<Point>
{
    new Point { X = 2, Y = 2 },
    new Point { X = 3, Y = 3 },
    new Point { X = 4, Y = 4 }
};

foreach (var pt in myListOfPoints)
{
    Console.WriteLine(pt);
}
```

Again, the benefit of this syntax is that you save yourself numerous keystrokes. While the nested curly brackets can become difficult to read if you don’t mind your formatting, imagine the amount of code that would be required to fill the following List<T> of Rectangles if you did not have collection initialization syntax.

```
List<Rectangle> myListOfRects = new List<Rectangle>
{
    new Rectangle {
        Height = 90, Width = 90,
        Location = new Point { X = 10, Y = 10 }},
    new Rectangle {
        Height = 50, Width = 50,
        Location = new Point { X = 2, Y = 2 }},
};
foreach (var r in myListOfRects)
{
    Console.WriteLine(r);
}
```