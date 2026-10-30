
{
    public string Make { get; init; }
    public string Model { get; init; }
    public string Color { get; init; }

    public Car(string make, string model, string color)
    {
        Make = make;
        Model = model;
        Color = color;
    }
}

//MiniVan.cs
namespace RecordInheritance
{
    //MiniVan record type
    public sealed record MiniVan : Car
    {
        public int Seating { get; init; }
        public MiniVan(string make, string model, string color, int seating) : base(make, model, color)
        {
            Seating = seating;
        }
    }
}

Notice that there isn’t much difference between these examples using record types and the previous examples using classes. The protected access modifiers on properties and methods behave the same, and the sealed access modifier on the record type prevents other record types from deriving from the sealed record types. You will also find the remaining topics in this chapter work with inherited record types as well. This is because record types are just a special type of immutable class (as detailed in Chapter 5).

Record types also include implicit casts to their base class, as shown in the following code:

```
using System;
using RecordInheritance;

Console.WriteLine("Record type inheritance!");

Car c = new Car("Honda","Pilot","Blue");
MiniVan m = new MiniVan("Honda", "Pilot", "Blue",10);
Console.WriteLine($"Checking MiniVan is-a Car:{m is Car}");
```

As one would expect, the output from the check m is that Car returns true, as the following output shows:

```
Record type inheritance!
Checking minivan is-a car:True
```

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

## Equality with Inherited Record Types

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

Record type inheritance!
MotorCycle and Scooter are equal: False

## Programming for Containment/Delegation

Recall that code reuse comes in two flavors. You have just explored the classical “is-a” relationship. Before you examine the third pillar of OOP (polymorphism), let’s examine the “has-a” relationship (also known as the containment/delegation model or aggregation). Returning to the Employees project, create a new file named BenefitPackage.cs and add the code to model an employee benefits package, as follows:

```
namespace Employees
{
    // This new type will function as a contained class.
    class BenefitPackage
    {
        // Assume we have other members that represent
        // dental/health benefits, and so on.
        public double ComputePayDeduction()
        {
            return 125.0;
        }
    }
}
```

Obviously, it would be rather odd to establish an “is-a” relationship between the BenefitPackage class and the employee types. (Employee “is-a” BenefitPackage? I don’t think so.) However, it should be clear that some sort of relationship between the two could be established. In short, you would like to express the idea that each employee “has-a” BenefitPackage. To do so, you can update the Employee class definition as follows:

```
// Employees now have benefits.
partial class Employee
{
    // Contain a BenefitPackage object.
    protected BenefitPackage EmpBenefits = new BenefitPackage();
    ...
}
```

At this point, you have successfully contained another object. However, exposing the functionality of the contained object to the outside world requires delegation. Delegation is simply the act of adding public members to the containing class that use the contained object’s functionality.

For example, you could update the Employee class to expose the contained empBenefits object using a custom property, as well as make use of its functionality internally using a new method named GetBenefitCost().

```
partial class Employee
{
    // Contain a BenefitPackage object.
    protected BenefitPackage EmpBenefits = new BenefitPackage();

// Expose certain benefit behaviors of object.
public double GetBenefitCost()
    => EmpBenefits.ComputePayDeduction();

// Expose object through a custom property.
public BenefitPackage Benefits
{
    get { return EmpBenefits; }
    set { EmpBenefits = value; }
}
```

In the following updated code, notice how you can interact with the internal BenefitsPackage type defined by the Employee type:

```
Console.WriteLine("***** The Employee Class Hierarchy ****\n");
...
Manager chucky = new Manager("Chucky", 50, 92, 100000, "333-23-2322", 9000);
double cost = chucky.GetBenefitCost();
Console.WriteLine($"Benefit Cost: {cost}");
Console.ReadLine();
```

## Understanding Nested Type Definitions

Chapter 5 briefly mentioned the concept of nested types, which is a spin on the “has-a” relationship you have just examined. In C# (as well as other .NET languages), it is possible to define a type (enum, class, interface, struct, or delegate) directly within the scope of a class or structure. When you have done so, the nested (or “inner”) type is considered a member of the nesting (or “outer”) class and in the eyes of the runtime can be manipulated like any other member (fields, properties, methods, and events). The syntax used to nest a type is quite straightforward.

```
public class OuterClass
{
    // A public nested type can be used by anybody.
    public class PublicInnerClass {}

    // A private nested type can only be used by members
    // of the containing class.
    private class PrivateInnerClass {}
}
```

Although the syntax is fairly clear, understanding why you would want to do this might not be readily apparent. To understand this technique, ponder the following traits of nesting a type:

- Nested types allow you to gain complete control over the access level of the inner type because they may be declared privately (recall that non-nested classes cannot be declared using the private keyword).
- Because a nested type is a member of the containing class, it can access private members of the containing class.
- Often, a nested type is useful only as a helper for the outer class and is not intended for use by the outside world.
