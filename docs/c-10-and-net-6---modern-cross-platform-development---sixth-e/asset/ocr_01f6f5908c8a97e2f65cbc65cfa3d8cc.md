List<Person> is read aloud as "list of Person," for example, "the type of the property named Children is a list of Person instances." We explicitly changed the class library to target .NET Standard 2.0 (that uses the C# 7 compiler), so we cannot use target-typed new to initialize the Children field. If we had left it targeting .NET 6.0, then we could use target-typed new, as shown in the following code:

```
public List<Person> Children = new();
```

We must ensure the collection is initialized to a new instance of a list of Person before we can add items to it, otherwise, the field will be null and it will throw runtime exceptions when we try to use any of its members like Add.

# Understanding generic collections

The angle brackets in the List<T> type is a feature of C# called generics that was introduced in 2005 with C# 2.0. It's a fancy term for making a collection strongly typed, that is, the compiler knows specifically what type of object can be stored in the collection. Generics improve the performance and correctness of your code.

Strongly typed has a different meaning to statically typed. The old System.Collection types are statically typed to contain weakly typed System.Object items. The newer System.Collection.Generic types are statically typed to contain strongly typed <T> instances.

Ironically, the term generics means we can use a more specific static type!

+   1. In Program.cs, add statements to add two children for Bob and then show how many children he has and what their names are, as shown in the following code:

```
bob.Children.Add(new Person { Name = "Alfred" }); // C# 3.0 and Later
bob.Children.Add(new() { Name = "Zoe" }); // C# 9.0 and Later

WriteLine(
    $"{bob.Name} has {bob.Children.Count} children:");

for (int childIndex = 0; childIndex < bob.Children.Count; childIndex++)
{
    WriteLine($"    {bob.Children[childIndex].Name}");
}
```

We could also use a foreach statement to enumerate over the collection. As an extra challenge, change the for statement to output the same information using foreach.

+   2. Run the code and view the result, as shown in the following output:

```
Bob Smith has 2 children:
    Alfred
    Zoe
```