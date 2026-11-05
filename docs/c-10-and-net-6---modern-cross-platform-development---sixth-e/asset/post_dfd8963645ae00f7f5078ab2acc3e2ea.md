
// Answer returned as an array of strings containing Pam and Jim
string[] result1 = query1.ToArray();

// Answer returned as a list of strings containing Pam and Jim
List<string> result2 = query2.ToList();

// Answer returned as we enumerate over the results
foreach (string name in query1)
{
    WriteLine(name); // outputs Pam
    names[2] = "Jimmy"; // change Jim to Jimmy
    // on the second iteration Jimmy does not end with an M
}

- 6. Run the console app and note the result, as shown in the following output:

```
Deferred execution
Pam
```

Due to deferred execution, after outputting the first result, Pam, if the original array values change, then by the time we loop back around, there are no more matches because Jim has become Jimmy and does not end with an M, so only Pam is outputted.

Before we get too deep into the weeds, let's slow down and look at some common LINQ extension methods and how to use them, one at a time.

## Filtering entities with Where

The most common reason for using LINQ is to filter items in a sequence using the Where extension method. Let's explore filtering by defining a sequence of names and then applying LINQ operations to it:

- 1. In the project file, comment out the element that enables implicit usings, as shown highlighted in the following markup:

```
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>net6.0</TargetFramework>
        <Nullable>enable</Nullable>
        <!-- <ImplicitUsings>enable</ImplicitUsings> -->
    </PropertyGroup>
</Project>
```

## Querying and Manipulating Data Using LINQ

2. In Program.cs, attempt to call the Where extension method on the array of names, as shown in the following code:

```
WriteLine("Writing queries");

var query = names.W
```

3. As you try to type the Where method, note that it is missing from the IntelliSense list of members of a string array, as shown in Figure 11.1:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_502_0.png)

Figure 11.1: IntelliSense with the Where extension method missing

This is because Where is an extension method. It does not exist on the array type. To make the Where extension method available, we must import the System.Linq namespace. This is implicitly imported by default in new .NET 6 projects, but we disabled it.

4. In the project file, uncomment out the element that enables implicit usings.

5. Retype the Where method and note that the IntelliSense list now includes the extension methods added by the Enumerable class, as shown in Figure 11.2:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_502_1.png)

Figure 11.2: IntelliSense showing LINQ Enumerable extension methods now

6. As you type the parentheses for the Where method, IntelliSense tells us that to call Where, we must pass in an instance of a Func<string, bool> delegate.

7. Enter an expression to create a new instance of a Func<string, bool> delegate, and for now note that we have not yet supplied a method name because we will define it in the next step, as shown in the following code:

```
var query = names.Where(new Func<string, bool>( ))
```

The Func<string, bool> delegate tells us that for each string variable passed to the method, the method must return a bool value. If the method returns true, it indicates that we should include the string in the results, and if the method returns false, it indicates that we should exclude it.

## Targeting a named method

Let's define a method that only includes names that are longer than four characters:

1. At the bottom of Program.cs, define a method that will include only names longer than four characters, as shown in the following code:

```
static bool NameLongerThanFour(string name)
{
    return name.Length > 4;
}
```

2. Above the NameLongerThanFour method, pass the method's name into the Func<string, bool> delegate, and then loop through the query items, as shown highlighted in the following code:

```
var query = names.Where(
    new Func<string, bool>(NameLongerThanFour));

foreach (string item in query)
{
    WriteLine(item);
}
```

3. Run the code and view the results, noting that only names longer than four letters are listed, as shown in the following output:

```
Writing queries
Michael
Dwight
Angela
Kevin
Creed
```

## Simplifying the code by removing the explicit delegate instantiation

We can simplify the code by deleting the explicit instantiation of the Func<string, bool> delegate because the C# compiler can instantiate the delegate for us:

- 1. To help you learn by seeing progressively improved code, copy and paste the query
- 2. Comment out the first example, as shown in the following code:
- 3. Modify the copy to remove the explicit instantiation of the delegate, as shown in the following code:
- 4. Run the code and note that it has the same behavior.

```
// var query = names.Where(
//   new Func<string, bool>(NameLongerThanFour));
```

```
var query = names.Where(NameLongerThanFour);
```

## Targeting a lambda expression

We can simplify our code even further using a lambda expression in place of a named method.

Although it can look complicated at first, a lambda expression is simply a nameless function. It uses the => (read as "goes to") symbol to indicate the return value:

- 1. Copy and paste the query, comment the second example, and modify the query, as shown in the following code:

```
var query = names.Where(name => name.Length > 4);
```

Note that the syntax for a lambda expression includes all the important parts of the NameLongerThanFour method, but nothing more. A lambda expression only needs to define the following:

- - The names of input parameters: name
- - A return value expression: name.Length > 4

The type of the name input parameter is inferred from the fact that the sequence contains string values, and the return type must be a bool value as defined by the delegate for Where to work, so the expression after the => symbol must return a bool value.

The compiler does most of the work for us, so our code can be as concise as possible.

- 2. Run the code and note that it has the same behavior.

## Sorting entities

Other commonly used extension methods are OrderBy and ThenBy, used for sorting a sequence.

Extension methods can be chained if the previous method returns another sequence, that is, a type that implements the IEnumerable<T> interface.

## Sorting by a single property using OrderBy

Let's continue working with the current project to explore sorting:

1. Append a call to OrderBy to the end of the existing query, as shown in the following code:

```
var query = names
.Where(name => name.Length > 4)
.OrderBy(name => name.Length);
```

> **Good Practice:** Format the LINQ statement so that each extension method call happens on its own line to make them easier to read.

2. Run the code and note that the names are now sorted by shortest first, as shown in the following output:

```
Kevin
Creed
Dwight
Angela
Michael
```

To put the longest name first, you would use OrderByDescending.

## Sorting by a subsequent property using ThenBy

We might want to sort by more than one property, for example, to sort names of the same length in alphabetical order:

1. Add a call to the ThenBy method at the end of the existing query, as shown highlighted in the following code:

```
var query = names
.Where(name => name.Length > 4)
.OrderBy(name => name.Length)
.ThenBy(name => name);
```

2. Run the code and note the slight difference in the following sort order. Within a group of names of the same length, the names are sorted alphabetically by the full value of the string, so Creed comes before Kevin, and Angela comes before Dwight, as shown in the following output:

```
Creed
Kevin
Angela
Dwight
Michael
```

## Declaring a query using var or a specified type

While writing a LINQ expression it is convenient to use var to declare the query object. This is because the type frequently changes as you work on the LINQ expression. For example, our query started as an IEnumerable<string> and is currently an IOrderedEnumerable<string>:

- 1. Hover your mouse over the var keyword and note that its type is IOrderedEnumerable<string>
- 2. Replace var with the actual type, as shown highlighted in the following code:

```
IOrderedEnumerable<string> query = names
.Where(name => name.Length > 4)
.OrderBy(name => name.Length)
.ThenBy(name => name);
```

> **Good Practice:** Once you have finished working on a query, you could change the declared type from var to the actual type to make it clearer what the type is. This is easy because your code editor can tell you what it is.

## Filtering by type

The Where extension method is great for filtering by values, such as text and numbers. But what if the sequence contains multiple types, and you want to filter by a specific type and respect any inheritance hierarchy?

Imagine that you have a sequence of exceptions. There are hundreds of exception types that form a complex hierarchy, as partially shown in Figure 11.3:
