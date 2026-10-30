```
int temp = a;
    a = b;
    b = temp;
}
}
```

So far, so good. But now assume you also need to swap two Person objects; this would require authoring a new version of Swap().

```
// Swap two Person objects.
static void Swap(ref Person a, ref Person b)
{
    Person temp = a;
    a = b;
    b = temp;
}
```

No doubt, you can see where this is going. If you also needed to swap floating-point numbers, bitmaps, cars, buttons, etc., you would have to build even more methods, which would become a maintenance nightmare. You could build a single (nongeneric) method that operated on object parameters, but then you face all the issues you examined earlier in this chapter, including boxing, unboxing, a lack of type safety, explicit casting, and so on.

Whenever you have a group of overloaded methods that differ only by incoming arguments, this is your clue that generics could make your life easier. Consider the following generic Swap<T>() method that can swap any two Ts:

```
// This method will swap any two items.
// as specified by the type parameter <T>.
static void Swap<T>(ref T a, ref T b)
{
    Console.WriteLine("You sent the Swap() method a {0}", typeof(T));
    T temp = a;
    a = b;
    b = temp;
}
```

Notice how a generic method is defined by specifying the type parameters after the method name but before the parameter list. Here, you state that the Swap<T>() method can operate on any two parameters of type <T>. To spice things up a bit, you also print out the type name of the supplied placeholder to the console using C#’s typeof() operator. Now consider the following calling code, which swaps integers and strings:

```
Console.WriteLine("***** Fun with Custom Generic Methods *****
");

// Swap 2 ints.
int a = 10, b = 90;
Console.WriteLine("Before swap: {0}, {1}", a, b);
SwapFunctions.Swap<int>(ref a, ref b);
Console.WriteLine("After swap: {0}, {1}", a, b);
Console.WriteLine();
```