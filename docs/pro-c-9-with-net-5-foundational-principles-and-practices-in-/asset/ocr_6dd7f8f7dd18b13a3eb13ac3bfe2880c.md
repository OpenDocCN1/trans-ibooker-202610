explicitly calling the six-parameter constructor defined by Employee and saving yourself unnecessary calls during the creation of the child class. Additionally, you added a specific behavior to the Manager class, in that the pay type is always set to Salaried. The custom SalesPerson constructor looks almost identical, with the exception that the pay type is set to Commission.

```
// As a general rule, all subclasses should explicitly call an appropriate
// base class constructor.
public SalesPerson(string fullName, int age, int empId,
    float currPay, string ssn, int numbOfSales)
: base(fullName, age, empId, currPay, ssn,
        EmployeePayTypeEnum.Commission)
{
    // This belongs with us!
    SalesNumber = numbOfSales;
}
```

> Note You may use the base keyword whenever a subclass wants to access a public or protected member defined by a parent class. Use of this keyword is not limited to constructor logic. You will see examples using base in this manner during the examination of polymorphism, later in this chapter.

Finally, recall that once you add a custom constructor to a class definition, the default constructor is silently removed. Therefore, be sure to redefine the default constructor for the SalesPerson and Manager types. Here’s an example:

```
// Add back the default ctor
// in the Manager class as well.
public SalesPerson() {}
```

# Keeping Family Secrets: The protected Keyword

As you already know, public items are directly accessible from anywhere, while private items can be accessed only by the class that has defined them. Recall from Chapter 5 that C# takes the lead of many other modern object languages and provides an additional keyword to define member accessibility: protected.

When a base class defines protected data or protected members, it establishes a set of items that can be accessed directly by any descendant. If you want to allow the SalesPerson and Manager child classes to directly access the data sector defined by Employee, you can update the original Employee class definition (in the EmployeeCore.cs file) as follows:

```
// Protected state data.
partial class Employee
{
    // Derived classes can now directly access this information.
    protected string EmpName;
    protected int EmpId;
    protected float CurrPay;
    protected int EmpAge;
    protected string EmpSsn;
    protected EmployeePayTypeEnum EmpPayType;...
}
```