
If you want to be explicit, you could add these keywords yourself with no ill effect (beyond a few additional keystrokes).

```
// An internal class with a private default constructor.
internal class Radio
{
    private Radio(){}
}
```

To allow other parts of a program to invoke members of an object, you must define them with the public keyword (or possibly with the protected keyword, which you will learn about in the next chapter). As well, if you want to expose the Radio to external assemblies (again, useful when building larger solutions or code libraries), you will need to add the public modifier.

```
// A public class with a public default constructor.
public class Radio
{
    public Radio(){}
}
```

## Using Access Modifiers and Nested Types

As mentioned in Table 5-1, the private, protected, protected internal, and private protected access modifiers can be applied to a nested type. Chapter 6 will examine nesting in detail. What you need to know at this point, however, is that a nested type is a type declared directly within the scope of class or structure. By way of example, here is a private enumeration (named CarColor) nested within a public class (named SportsCar):

```
public class SportsCar
{
    // OK! Nested types can be marked private.
    private enum CarColor
    {
        Red, Green, Blue
    }
}
```

Here, it is permissible to apply the private access modifier on the nested type. However, non-nested types (such as the SportsCar) can be defined only with the public or internal modifier. Therefore, the following class definition is illegal:

```
// Error! Nonnested types cannot be marked private!
private class SportsCar
{}
```

## Understanding the First Pillar: C#’s Encapsulation Services

The concept of encapsulation revolves around the notion that an object’s data should not be directly accessible from an object instance. Rather, class data is defined as private. If the object user wants to alter the state of an object, it does so indirectly using public members. To illustrate the need for encapsulation services, assume you have created the following class definition:

```
// A class with a single public field.
class Book
{
    public int numberOfPages;
}
```

The problem with public data is that the data itself has no ability to “understand” whether the current value to which it is assigned is valid with regard to the current business rules of the system. As you know, the upper range of a C# int is quite large (2,147,483,647). Therefore, the compiler allows the following assignment:

```
// Humm. That is one heck of a mini-novel!
Book miniNovel = new Book();
miniNovel.numberOfPages = 30_000_000;
```

Although you have not overflowed the boundaries of an int data type, it should be clear that a mini-novel with a page count of 30,000,000 pages is a bit unreasonable. As you can see, public fields do not provide a way to trap logical upper (or lower) limits. If your current system has a business rule that states a book must be between 1 and 1,000 pages, you are at a loss to enforce this programmatically. Because of this, public fields typically have no place in a production-level class definition.

> Note To be more specific, members of a class that represent an object's state should not be marked as public. As you will see later in this chapter, public constants and public read-only fields are quite useful.

Encapsulation provides a way to preserve the integrity of an object’s state data. Rather than defining public fields (which can easily foster data corruption), you should get in the habit of defining private data, which is indirectly manipulated using one of two main techniques.

- You can define a pair of public accessor (get) and mutator (set) methods.
- You can define a public property.

Whichever technique you choose, the point is that a well-encapsulated class should protect its data and hide the details of how it operates from the prying eyes of the outside world. This is often termed black-box programming. The beauty of this approach is that an object is free to change how a given method is implemented under the hood. It does this without breaking any existing code making use of it, provided that the parameters and return values of the method remain constant.

## Encapsulation Using Traditional Accessors and Mutators

Over the remaining pages in this chapter, you will be building a fairly complete class that models a general employee. To get the ball rolling, create a new Console Application project named EmployeeApp and create a new class file named Employee.cs. Update the Employee class with the following namespace, fields, methods, and constructors:

```
using System;
namespace EmployeeApp
{
class Employee
{
    // Field data.
    private string _empName;
    private int _empId;
    private float _currPay;

    // Constructors.
    public Employee() {}
    public Employee(string name, int id, float pay)
    {
        _empName = name;
        _empId = id;
        _currPay = pay;
    }

    // Methods.
    public void GiveBonus(float amount) => _currPay += amount;
    public void DisplayStats()
    {
        Console.WriteLine("Name: {0}", _empName);
        Console.WriteLine("ID: {0}", _empId);
        Console.WriteLine("Pay: {0}", _currPay);
    }
}
```

Notice that the fields of the Employee class are currently defined using the private keyword. Given this, the _empName, _empId, and _currPay fields are not directly accessible from an object variable. Therefore, the following logic in your code would result in compiler errors:

```
Employee emp = new Employee();
// Error! Cannot directly access private members
// from an object!
emp._empName = "Marv";
```

If you want the outside world to interact with a worker’s full name, a traditional approach is to define an accessor (get method) and a mutator (set method). The role of a get method is to return to the caller the current value of the underlying state data. A set method allows the caller to change the current value of the underlying state data, as long as the defined business rules are met.

To illustrate, let’s encapsulate the empName field. To do so, add the following public methods to the Employee class. Notice that the SetName() method performs a test on the incoming data to ensure the string is 15 characters or less. If it is not, an error prints to the console and returns without making a change to the empName field.

Note If this were a production-level class, you would also check the character length for an employee’s name within your constructor logic. Ignore this detail for the time being, as you will clean up this code in just a bit when you examine property syntax.

```
class Employee
{
    // Field data.
    private string _empName;
    ...

    // Accessor (get method).
    public string GetName() => _empName;

    // Mutator (set method).
    public void SetName(string name)
    {
        // Do a check on incoming value
        // before making assignment.
        if (name.Length > 15)
        {
            Console.WriteLine("Error! Name length exceeds 15 characters!");
        }
        else
        {
            _empName = name;
        }
    }
}
```

This technique requires two uniquely named methods to operate on a single data point. To test your new methods, update your code method as follows:

```
Console.WriteLine("***** Fun with Encapsulation ****\n");
Employee emp = new Employee("Marvin", 456, 30_000);
emp.GiveBonus(1000);
emp.DisplayStats();

// Use the get/set methods to interact with the object's name.
emp.SetName("Marv");
Console.WriteLine("Employee is named: {0}", emp.GetName());
Console.ReadLine();
```

Because of the code in your SetName() method, if you attempted to specify more than 15 characters (see the following), you would find the hard-coded error message printed to the console:

```
Console.WriteLine("***** Fun with Encapsulation ****\n");
...
// Longer than 15 characters! Error will print to console.
Employee emp2 = new Employee();
emp2.SetName("Xena the warrior princess");

Console.ReadLine();
```

So far, so good. You have encapsulated the private empName field using two public methods named GetName() and SetName(). If you were to further encapsulate the data in the Employee class, you would need
