```
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
```

+   6. Run the console app and note the result, as shown in the following output:

```
Deferred execution
Pam
```

Due to deferred execution, after outputting the first result, Pam, if the original array values change, then by the time we loop back around, there are no more matches because Jim has become Jimmy and does not end with an M, so only Pam is outputted.

Before we get too deep into the weeds, let's slow down and look at some common LINQ extension methods and how to use them, one at a time.

# Filtering entities with Where

The most common reason for using LINQ is to filter items in a sequence using the Where extension method. Let's explore filtering by defining a sequence of names and then applying LINQ operations to it:

+   1. In the project file, comment out the element that enables implicit usings, as shown highlighted in the following markup:

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