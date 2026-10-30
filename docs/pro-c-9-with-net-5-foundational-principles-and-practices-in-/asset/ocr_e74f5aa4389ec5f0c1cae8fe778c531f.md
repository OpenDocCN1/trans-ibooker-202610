These examples should give you enough knowledge to feel comfortable with the process of building LINQ query expressions. While there are additional operators you have not yet examined, you will see further examples later in this text when you learn about related LINQ technologies. To wrap up your first look at LINQ, the remainder of this chapter will dive into the details between the C# LINQ query operators and the underlying object model.

# The Internal Representation of LINQ Query Statements

At this point, you have been introduced to the process of building query expressions using various C# query operators (such as from, in, where, orderby, and select). Also, you discovered that some functionality of the LINQ to Objects API can be accessed only when calling extension methods of the Enumerable class. The truth of the matter, however, is that when LINQ queries are compiled, the C# compiler translates all C# LINQ operators into calls on methods of the Enumerable class.

A great many of the methods of Enumerable have been prototyped to take delegates as arguments. Many methods require a generic delegate named Func<>, which was introduced to you during your examination of generic delegates in Chapter 10. Consider the where() method of Enumerable, which is called on your behalf when you use the C# where LINQ query operator.

```
// Overloaded versions of the Enumerable.Where<T>() method.
// Note the second parameter is of type System.Func<>.
public static IEnumerable<TSource> Where<TSource>(
    this IEnumerable<TSource> source,
    System.Func<TSource,int,bool> predicate)

public static IEnumerable<TSource> Where<TSource>(
    this IEnumerable<TSource> source,
    System.Func<TSource,bool> predicate)
```

The Func<> delegate (as the name implies) represents a pattern for a given function with a set of up to 16 arguments and a return value. If you were to examine this type using the Visual Studio Object Browser, you would notice various forms of the Func<> delegate. Here’s an example:

```
// The various formats of the Func<> delegate.
public delegate TResult Func<T1,T2,T3,T4,TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4)

public delegate TResult Func<T1,T2,T3,TResult>(T1 arg1, T2 arg2, T3 arg3)

public delegate TResult Func<T1,T2,TResult>(T1 arg1, T2 arg2)

public delegate TResult Func<T1,TResult>(T1 arg1)

public delegate TResult Func<TResult>()
```

Given that many members of System.Linq.Enumerable demand a delegate as input, when invoking them, you can either manually create a new delegate type and author the necessary target methods, use a C# anonymous method, or define a proper lambda expression. Regardless of which approach you take, the result is identical.

While it is true that using C# LINQ query operators is far and away the simplest way to build a LINQ query expression, let’s walk through each of these possible approaches, just so you can see the connection between the C# query operators and the underlying Enumerable type.