The output is as you would expect.

```
**** A First Look at Structures ****

X = 349, Y = 76
X = 350, Y = 77
```

# Creating Structure Variables

When you want to create a structure variable, you have a variety of options. Here, you simply create a Point variable and assign each piece of public field data before invoking its members. If you do not assign each piece of public field data (X and Y in this case) before using the structure, you will receive a compiler error.

```
// Error! Did not assign Y value.
Point p1;
p1.X = 10;
p1.Display();
```

```
// OK! Both fields assigned before use.
Point p2;
p2.X = 10;
p2.Y = 10;
p2.Display();
```

As an alternative, you can create structure variables using the C# new keyword, which will invoke the structure's default constructor. By definition, a default constructor does not take any arguments. The benefit of invoking the default constructor of a structure is that each piece of field data is automatically set to its default value.

```
// Set all fields to default values
// using the default constructor.
Point p1 = new Point();

// Prints X=0,Y=0.
p1.Display();
```

It is also possible to design a structure with a custom constructor. This allows you to specify the values of field data upon variable creation, rather than having to set each data member field by field. Chapter 5 will provide a detailed examination of constructors; however, to illustrate, update the Point structure with the following code:

```
struct Point
{
    // Fields of the structure.
    public int X;
    public int Y;

    // A custom constructor.
    public Point(int xPos, int yPos)
    {
```