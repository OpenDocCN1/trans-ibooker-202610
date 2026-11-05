# Controlling access with properties and indexers

Earlier, you created a method named GetOrigin that returned a string containing the name and origin of the person. Languages such as Java do this a lot. C# has a better way: properties.

A property is simply a method (or a pair of methods) that acts and looks like a field when you want to get or set a value, thereby simplifying the syntax.

# Defining read-only properties

A readonly property only has a get implementation.

+   1. In PersonAutoGen.cs, in the Person class, add statements to define three properties:
+      1. The first property will perform the same role as the GetOrigin method using the property syntax that works with all versions of C# (although, it uses the string interpolation syntax from C# 6 and later).
+      2. The second property will return a greeting message using the lambda expression body => syntax from C# 6 and later.
+      3. The third property will calculate the person's age.

Here's the code:

```
```csharp
// a property defined using C# 1 - 5 syntax
public string Origin
{
    get
    {
        return $"{Name} was born on {HomePlanet}";
    }
}

// two properties defined using C# 6+ lambda expression body syntax
public string Greeting => $"{Name} says 'Hello!'";

public int Age => System.DateTime.Today.Year - DateOfBirth.Year;
```
```

> **Good Practice:** This isn't the best way to calculate someone's age, but we aren't learning how to calculate an age from a date of birth. If you need to do that properly, read the discussion at the following link: https://stackoverflow.com/questions/9/how-do-i-calculate-someones-age-in-c