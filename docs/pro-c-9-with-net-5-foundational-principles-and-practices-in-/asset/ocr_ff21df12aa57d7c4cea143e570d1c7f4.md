It’s important to note that even though record types are specialized classes, you cannot cross-inherit between classes and records. To be clear, classes cannot inherit from record types, and record types cannot inherit from classes. Consider the following code, and notice that the last two examples won’t compile:

```
namespace RecordInheritance
{
    public class TestClass { }
    public record TestRecord { }

    //Classes cannot inherit records
    // public class Test2 : TestRecord { }

    //Records types cannot inherit from classes
    // public record Test2 : TestClass { }
}
```

Inheritance also works with positional record types. Create a new file named PositionalRecordTypes.cs in your project. Add the following code into your file:

```
namespace RecordInheritance
{
    public record PositionalCar (string Make, string Model, string Color);
    public record PositionalMiniVan (string Make, string Model, string Color)
        : PositionalCar(Make, Model, Color);
}
```

Add the following the code to show what you already know to be true, that the positional record types work exactly the same as record types:

```
PositionalCar pc = new PositionalCar("Honda", "Pilot", "Blue");
PositionalMiniVan pm = new PositionalMiniVan("Honda", "Pilot", "Blue", 10);
Console.WriteLine($"Checking PositionalMiniVan is-a PositionalCar:{pm is PositionalCar}");
```

# Equality with Inherited Record Types

Recall from Chapter 5 that record types use value semantics to determine equality. One additional detail regarding record types is that the type of the record is part of the equality consideration. For example, consider the following trivial examples:

```
public record MotorCycle(string Make, string Model);
public record Scooter(string Make, string Model) : MotorCycle(Make,Model);
```

Ignoring the fact that typically inherited classes extend base classes, these simple examples define two different record types that have the same properties. When creating instances with the same values for the properties, they fail the equality test due to being different types. Take the following code and results for example:

```
MotorCycle mc = new MotorCycle("Harley","Lowrider");
Scooter sc = new Scooter("Harley", "Lowrider");
Console.WriteLine($"MotorCycle and Scooter are equal: {Equals(mc,sc)}");
```