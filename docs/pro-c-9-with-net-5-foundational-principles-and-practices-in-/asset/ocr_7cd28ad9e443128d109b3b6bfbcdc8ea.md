Here, each item that matches the search criteria has been given the name g (as in “game”); however, any valid C# variable name would do.

```
IEnumerable<string> subset =
    from game in currentVideoGames
    where game.Contains(" ")
    orderby game
    select game;
```

Notice that the returned sequence is held in a variable named subset, typed as a type that implements the generic version of IEnumerable<T>, where T is of type System.String (after all, you are querying an array of strings). After you obtain the result set, you then simply print out each item using a standard foreach construct. If you run your application, you will find the following output:

```
***** Fun with LINQ to Objects *****
Item: Fallout 3
Item: System Shock 2
Item: Uncharted 2
```

# Once Again, Using Extension Methods

The LINQ syntax used earlier (and the rest of this chapter) is referred to as LINQ query expressions, which is a format that is like SQL but slightly different. There is another syntax that uses extension methods that will be the syntax used in most of the examples in this book.

Create a new method named QueryOverStringsWithExtensionMethods() and enter the following code:

```
static void QueryOverStringsWithExtensionMethods()
{
    // Assume we have an array of strings.
    string[] currentVideoGames = {"Morrowind", "Uncharted 2", "Fallout 3", "Daxter", "System Shock 2"};

    // Build a query expression to find the items in the array
    // that have an embedded space.
    IEnumerable<string> subset =
        currentVideoGames.Where(g => g.Contains(" ")).OrderBy(g => g).Select(g => g);

    // Print out the results.
    foreach (string s in subset)
    {
        Console.WriteLine("Item: {0}", s);
    }
}
```

Everything is the same as the previous method, except for the line in bold. This is using the extension method syntax. This syntax uses lambda expressions within each method to define the operation. For example, the lambda in the Where() method defines the condition (where a value contains a space). Just as in the query expression syntax, the letter used to indicate the value being evaluated in the lambda is arbitrary; I could have used v for video games.