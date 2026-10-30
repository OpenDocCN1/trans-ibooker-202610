# CHAPTER 10

# Collections and Generics

Any application you create with the .NET Core platform will need to contend with the issue of maintaining and manipulating a set of data points in memory. These data points can come from any variety of locations including a relational database, a local text file, an XML document, a web service call, or perhaps user-provided input.

When the .NET platform was first released, programmers frequently used the classes of the System.Collections namespace to store and interact with bits of data used within an application. In .NET 2.0, the C# programming language was enhanced to support a feature termed generics; with this change, a new namespace was introduced in the base class libraries: System.Collections.Generic.

This chapter will provide you with an overview of the various collection (generic and nongeneric) namespaces and types found within the .NET Core base class libraries. As you will see, generic containers are often favored over their nongeneric counterparts because they typically provide greater type safety and performance benefits. After you have learned how to create and manipulate the generic items found in the framework, the remainder of this chapter will examine how to build your own generic methods and generic types. As you do this, you will learn about the role of constraints (and the corresponding C# where keyword), which allow you to build extremely type-safe classes.

# The Motivation for Collection Classes

The most primitive container you could use to hold application data is undoubtedly the array. As you saw in Chapter 4, C# arrays allow you to define a set of identically typed items (including an array of System.Object, which essentially represents an array of any type of data) of a fixed upper limit. Also recall from Chapter 4 that all C# array variables gather a good deal of functionality from the System.Array class. By way of a quick review, consider the following code, which creates an array of textual data and manipulates its contents in various ways:

```
// Make an array of string data.
string[] strArray = {"First", "Second", "Third"};

// Show number of items in array using Length property.
Console.WriteLine("This array has {0} items.", strArray.Length);
Console.WriteLine();

// Display contents using enumerator.
foreach (string s in strArray)
{
    Console.WriteLine("Array Entry: {0}", s);
}
Console.WriteLine();
```