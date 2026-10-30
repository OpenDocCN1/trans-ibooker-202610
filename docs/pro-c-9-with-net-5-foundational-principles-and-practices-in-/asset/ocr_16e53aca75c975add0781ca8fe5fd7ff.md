```
// Assign incoming parameters using the
// inherited properties of the parent class.
Id = empId;
Age = age;
Name = fullName;
Pay = currPay;
PayType = EmployeePayTypeEnum.Salaried;
// OOPS! This would be a compiler error,
// if the SSN property were read-only!
SocialSecurityNumber = ssn;
}
```

The first issue with this approach is that if you defined any property as read-only (e.g., the SocialSecurityNumber property), you are unable to assign the incoming string parameter to this field, as shown in the final code statement of this custom constructor.

The second issue is that you have indirectly created a rather inefficient constructor, given that under C#, unless you say otherwise, the default constructor of a base class is called automatically before the logic of the derived constructor is executed. After this point, the current implementation accesses numerous public properties of the Employee base class to establish its state. Thus, you have really made eight hits (six inherited properties and two constructor calls) during the creation of a Manager object!

To help optimize the creation of a derived class, you will do well to implement your subclass constructors to explicitly call an appropriate custom base class constructor, rather than the default. In this way, you are able to reduce the number of calls to inherited initialization members (which saves processing time). First, ensure your Employee parent class has the following six-argument constructor:

```
// Add to the Employee base class.
public Employee(string name, int age, int id, float pay, string empSsn, EmployeePayTypeEnum payType)
{
    Name = name;
    Id = id;
    Age = age;
    Pay = pay;
    SocialSecurityNumber = empSsn;
    PayType = payType;
}
```

Now, let’s retrofit the custom constructor of the Manager type to call this constructor using the base keyword.

```
public Manager(string fullName, int age, int empId,
    float currPay, string ssn, int numbOfOpts)
    : base(fullName, age, empId, currPay, ssn,
        EmployeePayTypeEnum.Salaried)
{
    // This property is defined by the Manager class.
    StockOptions = numbOfOpts;
}
```

Here, the base keyword is hanging off the constructor signature (much like the syntax used to chain constructors on a single class using the this keyword, as was discussed in Chapter 5), which always indicates a derived constructor is passing data to the immediate parent constructor. In this situation, you are