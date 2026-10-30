New in C# 9.0, anonymous methods can also be marked as static to preserve encapsulation and ensure that the method cannot introduce any side effects into the containing code. For example, see the updated anonymous method here:

```
c1.AboutToBlow += static delegate
{
    //This causes a compile error because it is marked static
    aboutToBlowCounter++;
    Console.WriteLine("Eek! Going too fast!");
};
```

The preceding code will not compile due to the anonymous methods attempting to access the variable declared outside its scope.

# Discards with Anonymous Methods (New 9.0)

Discards, introduced in Chapter 3, have been updated in C# 9.0 for use as input parameters for anonymous methods, with a catch. Because the underscore (_) was a legal variable identifier in previous versions of C#, there must be two or more discards used with the anonymous method to be treated as discards.

For example, the following code created a delegate for a Func that takes two integers and returns another. This implementation ignores any variables passed in and returns 42:

```
Console.WriteLine("******** Discards with Anonymous Methods ********");

Func<int,int,int> constant = delegate (int _, int _) {return 42;};
Console.WriteLine("constant(3,4)={0}",constant(3,4));
```

# Understanding Lambda Expressions

To conclude your look at the .NET Core event architecture, you will examine C# lambda expressions. As just explained, C# supports the ability to handle events “inline” by assigning a block of code statements directly to an event using anonymous methods, rather than building a stand-alone method to be called by the underlying delegate. Lambda expressions are nothing more than a concise way to author anonymous methods and ultimately simplify how you work with the .NET Core delegate type.

To set the stage for your examination of lambda expressions, create a new Console Application project named LambdaExpressions. To begin, consider the FindAll() method of the generic List<T> class. This method can be called when you need to extract a subset of items from the collection and is prototyped like so:

```
// Method of the System.Collections.Generic.List<T>
public List<T> FindAll(Predicate<T> match)
```

As you can see, this method returns a new List<T> that represents the subset of data. Also notice that the sole parameter to FindAll() is a generic delegate of type System.Predicate<T>. This delegate type can point to any method returning a bool and takes a single type parameter as the only input parameter.

```
// This delegate is used by FindAll() method
// to extract out the subset.
public delegate bool Predicate<T>(T obj);
```