Of course, nothing is wrong with these approaches to data manipulation. In fact, you can (and will) certainly make direct use of ADO.NET, the XML namespaces, reflection services, and the various collection types. However, the basic problem is that each of these APIs is an island unto itself, which offers little in the way of integration. True, it is possible (for example) to save an ADO.NET DataSet as XML and then manipulate it via the System.Xml namespaces, but nonetheless, data manipulation remains rather asymmetrical.

The LINQ API is an attempt to provide a consistent, symmetrical way programmers can obtain and manipulate "data" in the broad sense of the term. Using LINQ, you can create directly within the C# programming language constructs called query expressions. These query expressions are based on numerous query operators that have been intentionally designed to look and feel similar (but not quite identical) to a SQL expression.

The twist, however, is that a query expression can be used to interact with numerous types of data—even data that has nothing to do with a relational database. Strictly speaking, "LINQ" is the term used to describe this overall approach to data access. However, based on where you are applying your LINQ queries, you will encounter various terms, such as the following:

+   - LINQ to Objects: This term refers to the act of applying LINQ queries to arrays and collections.
+   - LINQ to XML: This term refers to the act of using LINQ to manipulate and query XML documents.
+   - LINQ to Entities: This aspect of LINQ allows you to make use of LINQ queries within the ADO.NET Entity Framework (EF) Core API.
+   - Parallel LINQ (aka PLINQ): This allows for parallel processing of data returned from a LINQ query.

Today, LINQ is an integral part of the .NET Core base class libraries, managed languages, and Visual Studio itself.

# LINQ Expressions Are Strongly Typed

It is also important to point out that a LINQ query expression (unlike a traditional SQL statement) is strongly typed. Therefore, the C# compiler will keep you honest and make sure that these expressions are syntactically well formed. Tools such as Visual Studio can use metadata for useful features such as IntelliSense, autocompletion, and so forth.

# The Core LINQ Assemblies

To work with LINQ to Objects, you must make sure that every C# code file that contains LINQ queries imports the System.Linq namespace. Make sure the following using statement is in each code file that uses LINQ:

```
using System.Linq;
```

# Applying LINQ Queries to Primitive Arrays

To begin examining LINQ to Objects, let's build an application that will apply LINQ queries to various array objects. Create a Console Application project named LinqOverArray and define a static helper method within the Program class named QueryOverStrings(). In this method, create a string array containing six or so items of your liking (here, I listed a batch of video games in my library). Make sure to have at least two entries that contain numerical values and a few that have embedded spaces.