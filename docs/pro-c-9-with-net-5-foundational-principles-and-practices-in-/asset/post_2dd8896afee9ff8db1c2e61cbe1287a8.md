
When a type nests another class type, it can create member variables of the type, just as it would for any point of data. However, if you want to use a nested type from outside the containing type, you must qualify it by the scope of the nesting type. Consider the following code:

```
// Create and use the public inner class. OK!
OuterClass.PublicInnerClass inner;
inner = new OuterClass.PublicInnerClass();

// Compiler Error! Cannot access the private class.
OuterClass.PrivateInnerClass inner2;
inner2 = new OuterClass.PrivateInnerClass();
```

To use this concept within the employee’s example, assume you have now nested the BenefitPackage directly within the Employee class type.

```
partial class Employee
{
    public class BenefitPackage
    {
        // Assume we have other members that represent
        // dental/health benefits, and so on.
        public double ComputePayDeduction()
        {
            return 125.0;
        }
    }
    ...
}
```

The nesting process can be as “deep” as you require. For example, assume you want to create an enumeration named BenefitPackageLevel, which documents the various benefit levels an employee may choose. To programmatically enforce the tight connection between Employee, BenefitPackage, and BenefitPackageLevel, you could nest the enumeration as follows:

```
// Employee nests BenefitPackage.
public partial class Employee
{
    // BenefitPackage nests BenefitPackageLevel.
    public class BenefitPackage
    {
        public enum BenefitPackageLevel
        {
            Standard, Gold, Platinum
        }

        public double ComputePayDeduction()
        {
            return 125.0;
        }
    }
    ...
}
```

Because of the nesting relationships, note how you are required to make use of this enumeration:

```
...
// Define my benefit level.
Employee.BenefitPackage.BenefitPackageLevel myBenefitLevel =
    Employee.BenefitPackage.BenefitPackageLevel.Platinum;
```

Excellent! At this point, you have been exposed to a number of keywords (and concepts) that allow you to build hierarchies of related types via classical inheritance, containment, and nested types. If the details aren’t crystal clear right now, don’t sweat it. You will be building a number of additional hierarchies over the remainder of this book. Next up, let’s examine the final pillar of OOP: polymorphism.

## Understanding the Third Pillar of OOP: C#’s Polymorphic Support

Recall that the Employee base class defined a method named GiveBonus(), which was originally implemented as follows (before updating it to use the property pattern):

```
public partial class Employee
{
    public void GiveBonus(float amount) => _currPay += amount;
    ...
}
```

Because this method has been defined with the public keyword, you can now give bonuses to salespeople and managers (as well as part-time salespeople).

```
Console.WriteLine("***** The Employee Class Hierarchy *****
");

// Give each employee a bonus?
Manager chucky = new Manager("Chucky", 50, 92, 100000, "333-23-2322", 9000);
chucky.GiveBonus(300);
chucky.DisplayStats();
Console.WriteLine();

SalesPerson fran = new SalesPerson("Fran", 43, 93, 3000, "932-32-3232", 31);
fran.GiveBonus(200);
fran.DisplayStats();
Console.ReadLine();
```

The problem with the current design is that the publicly inherited GiveBonus() method operates identically for all subclasses. Ideally, the bonus of a salesperson or part-time salesperson should consider the number of sales. Perhaps managers should gain additional stock options in conjunction with a monetary bump in salary. Given this, you are suddenly faced with an interesting question: “How can related types respond differently to the same request?” Again, glad you asked!

## Using the virtual and override Keywords

Polymorphism provides a way for a subclass to define its own version of a method defined by its base class, using the process termed *method overriding*. To retrofit your current design, you need to understand the meaning of the virtual and override keywords. If a base class wants to define a method that *may be* (but does not have to be) overridden by a subclass, it must mark the method with the virtual keyword.

```
partial class Employee
{
    // This method can now be "overridden" by a derived class.
    public virtual void GiveBonus(float amount)
    {
        Pay += amount;
    }
    ...
}
```

> Note Methods that have been marked with the virtual keyword are (not surprisingly) termed *virtual methods.*

When a subclass wants to change the implementation details of a virtual method, it does so using the override keyword. For example, SalesPerson and Manager could override GiveBonus() as follows (assume that PTSalesPerson will not override GiveBonus() and, therefore, simply inherits the version defined by SalesPerson):

```
using System;
class SalesPerson : Employee
{
    ...
    // A salesperson's bonus is influenced by the number of sales.
    public override void GiveBonus(float amount)
    {
        int salesBonus = 0;
        if (SalesNumber >= 0 && SalesNumber <= 100)
            salesBonus = 10;
        else
        {
            if (SalesNumber >= 101 && SalesNumber <= 200)
                salesBonus = 15;
            else
                salesBonus = 20;
        }
        base.GiveBonus(amount * salesBonus);
    }
}
```

```
class Manager : Employee
{
    ...
    public override void GiveBonus(float amount)
    {
        base.GiveBonus(amount);
        Random r = new Random();
        StockOptions += r.Next(500);
    }
}
```

Notice how each overridden method is free to leverage the default behavior using the base keyword. In this way, you have no need to completely reimplement the logic behind GiveBonus() but can reuse (and possibly extend) the default behavior of the parent class.

Also assume that the current DisplayStats() method of the Employee class has been declared virtually.

```
public virtual void DisplayStats()
{
    Console.WriteLine("Name: {0}", Name);
    Console.WriteLine("Id: {0}", Id);
    Console.WriteLine("Age: {0}", Age);
    Console.WriteLine("Pay: {0}", Pay);
    Console.WriteLine("SSN: {0}", SocialSecurityNumber);
}
```

By doing so, each subclass can override this method to account for displaying the number of sales (for salespeople) and current stock options (for managers). For example, consider Manager’s version of the DisplayStats() method (the SalesPerson class would implement DisplayStats() in a similar manner to show the number of sales).

```
//Manager.cs
public override void DisplayStats()
{
    base.DisplayStats();
    Console.WriteLine("Number of Stock Options: {0}", StockOptions);
}
//SalesPerson.cs
public override void DisplayStats()
{
    base.DisplayStats();
    Console.WriteLine("Number of Sales: {0}", SalesNumber);
}
```

Now that each subclass can interpret what these virtual methods mean for itself, each object instance behaves as a more independent entity.

```
Console.WriteLine("***** The Employee Class Hierarchy *****
");

// A better bonus system!
Manager chucky = new Manager("Chucky", 50, 92, 100000, "333-23-2322", 9000);
chucky.GiveBonus(300);
chucky.DisplayStats();
Console.WriteLine();

SalesPerson fran = new SalesPerson("Fran", 43, 93, 3000, "932-32-3232", 31);
fran.GiveBonus(200);
fran.DisplayStats();
Console.ReadLine();
```

The following output shows a possible test run of your application thus far:

```
**** The Employee Class Hierarchy ****
Name: Chucky
ID: 92
Age: 50
Pay: 100300
SSN: 333-23-2322
Number of Stock Options: 9337

Name: Fran
ID: 93
Age: 43
Pay: 5000
SSN: 932-32-3232
Number of Sales: 31
```

## Overriding Virtual Members with Visual Studio/Visual Studio Code

As you might have already noticed, when you are overriding a member, you must recall the type of every parameter—not to mention the method name and parameter-passing conventions (ref, out, and params). Both Visual Studio and Visual Studio Code have a helpful feature that you can make use of when overriding a virtual member. If you type the word override within the scope of a class type (then hit the spacebar), IntelliSense will automatically display a list of all the overridable members defined in your parent classes, excluding methods already overridden.

When you select a member and hit the Enter key, the IDE responds by automatically filling in the method stub on your behalf. Note that you also receive a code statement that calls your parent’s version of the virtual member (you are free to delete this line if it is not required). For example, if you used this technique when overriding the DisplayStats() method, you might find the following autogenerated code:

```
public override void DisplayStats()
{
    base.DisplayStats();
}
```

## Sealing Virtual Members

Recall that the sealed keyword can be applied to a class type to prevent other types from extending its behavior via inheritance. As you might remember, you sealed PtSalesPerson because you assumed it made no sense for other developers to extend this line of inheritance any further.

On a related note, sometimes you might not want to seal an entire class but simply want to prevent derived types from overriding particular virtual methods. For example, assume you do not want part-time salespeople to obtain customized bonuses. To prevent the PTSalesPerson class from overriding the virtual GiveBonus() method, you could effectively seal this method in the SalesPerson class as follows:

```
// SalesPerson has sealed the GiveBonus() method!
class SalesPerson : Employee
{
...
}
