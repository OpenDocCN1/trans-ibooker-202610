

to add various additional methods (such as GetID(), SetID(), GetCurrentPay(), SetCurrentPay()). Each of the mutator methods could also have various lines of code to check for additional business rules. While this could certainly be done, the C# language has a useful alternative notation to encapsulate class data.

## Encapsulation Using Properties

Although you can encapsulate a piece of field data using traditional get and set methods, .NET Core languages prefer to enforce data encapsulation state data using properties. First, understand that properties are just a container for "real" accessor and mutator methods, named get and set, respectively. Therefore, as a class designer, you are still able to perform any internal logic necessary before making the value assignment (e.g., uppercase the value, scrub the value for illegal characters, check the bounds of a numerical value, etc.).

Here is the updated Employee class, now enforcing encapsulation of each field using property syntax rather than traditional get and set methods:

```
class Employee
{
    // Field data.
    private string _empName;
    private int _empId;
    private float _currPay;
    // Properties!
    public string Name
    {
        get { return _empName; }
        set
        {
            if (value.Length > 15)
            {
                Console.WriteLine("Error! Name length exceeds 15 characters!");
            }
            else
            {
                _empName = value;
            }
        }
    }
    // We could add additional business rules to the sets of these properties;
    // however, there is no need to do so for this example.
    public int Id
    {
        get { return _empId; }
        set { _empId = value; }
    }
    public float Pay
    {
        get { return _currPay; }
        set { _currPay = value; }
    }
    ...
}
```

A C# property is composed by defining a get scope (accessor) and set scope (mutator) directly within the property itself. Notice that the property specifies the type of data it is encapsulating by what appears to be a return value. Also take note that, unlike a method, properties do not make use of parentheses (not even empty parentheses) when being defined. Consider the following commentary on your current Id property:

```
// The 'int' represents the type of data this property encapsulates.
public int Id // Note lack of parentheses.
{
    get { return _empId; }
    set { _empId = value; }
}
```

Within a set scope of a property, you use a token named value, which is used to represent the incoming value used to assign the property by the caller. This token is not a true C# keyword but is what is known as a contextual keyword. When the token value is within the set scope of the property, it always represents the value being assigned by the caller, and it will always be the same underlying data type as the property itself. Thus, notice how the Name property can still test the range of the string as so:

```
public string Name
{
    get { return _empName; }
    set
    {
        // Here, value is really a string.
        if (value.Length > 15)
        {    Console.WriteLine("Error! Name length exceeds 15 characters!");
        }
        else
        {
            empName = value;
        }
    }
}
```

After you have these properties in place, it appears to the caller that it is getting and setting a public point of data; however, the correct get and set block is called behind the scenes to preserve encapsulation.

```
Console.WriteLine("**** Fun with Encapsulation ****\n");
Employee emp = new Employee("Marvin", 456, 30000);
emp.GiveBonus(1000);
emp.DisplayStats();
```

```
// Reset and then get the Name property.
emp.Name = "Marv";
Console.WriteLine("Employee is named: {0}", emp.Name);
Console.ReadLine();
```

Properties (as opposed to accessor and mutator methods) also make your types easier to manipulate, in that properties are able to respond to the intrinsic operators of C#. To illustrate, assume that the Employee

class type has an internal private member variable representing the age of the employee. Here is the relevant update (notice the use of constructor chaining):

```
class Employee
{
    ...
    // New field and property.
    private int _empAge;
    public int Age
    {
        get { return _empAge; }
        set { _empAge = value; }
    }

    // Updated constructors.
    public Employee() {}
    public Employee(string name, int id, float pay)
    :this(name, 0, id, pay){}

    public Employee(string name, int age, int id, float pay)
    {
        _empName = name;
        _empId = id;
        _empAge = age;
        _currPay = pay;
    }

    // Updated DisplayStats() method now accounts for age.
    public void DisplayStats()
    {
        Console.WriteLine("Name: {0}", _empName);
        Console.WriteLine("ID: {0}", _empId);
        Console.WriteLine("Age: {0}", _empAge);
        Console.WriteLine("Pay: {0}", _currPay);
    }
}
```

Now assume you have created an Employee object named joe. On his birthday, you want to increment the age by one. Using traditional accessor and mutator methods, you would need to write code such as the following:

```
Employee joe = new Employee();
joe.SetAge(joe.GetAge() + 1);
```

However, if you encapsulate empAge using a property named Age, you are able to simply write this:

```
Employee joe = new Employee();
joe.Age++;
```

## Properties As Expression-Bodied Members (New 7.0)

As mentioned previously, property get and set accessors can also be written as expression-bodied members. The rules and syntax are the same: single-line methods can be written using the new syntax. So, the Age property could be written like this:

```
public int Age
{
    get => empAge;
    set => empAge = value;
}
```

Both syntaxes compile down to the same IL, so which syntax you use is completely up to you. In this text, you will see a mix of both styles to keep visibility on them, not because I am adhering to a specific code style.

## Using Properties Within a Class Definition

Properties, specifically the set portion of a property, are common places to package up the business rules of your class. Currently, the Employee class has a Name property that ensures the name is no more than 15 characters. The remaining properties (ID, Pay, and Age) could also be updated with any relevant logic.

While this is well and good, also consider what a class constructor typically does internally. It will take the incoming parameters, check for valid data, and then make assignments to the internal private fields. Currently, your master constructor does not test the incoming string data for a valid range, so you could update this member as so:

```
public Employee(string name, int age, int id, float pay)
{
    // Humm, this seems like a problem...
    if (name.Length > 15)
    {
        Console.WriteLine("Error! Name length exceeds 15 characters!");
    }
    else
    {
        _empName = name;
    }
    _empId = id;
    _empAge = age;
    _currPay = pay;
}
```

I am sure you can see the problem with this approach. The Name property and your master constructor are performing the same error checking. If you were also making checks on the other data points, you would have a good deal of duplicate code. To streamline your code and isolate all of your error checking to a central location, you will do well if you always use properties within your class whenever you need to get or set the values. Consider the following updated constructor:

```
public Employee(string name, int age, int id, float pay)
{
    // Better! Use properties when setting class data.
    // This reduces the amount of duplicate error checks.
```

```
Name = name;
Age = age;
ID = id;
Pay = pay;
}
```

Beyond updating constructors to use properties when assigning values, it is good practice to use properties throughout a class implementation to ensure your business rules are always enforced. In many cases, the only time when you directly refer to the underlying private piece of data is within the property itself. With this in mind, here is your updated Employee class:

```
class Employee
{
    // Field data.
    private string _empName;
    private int _empId;
    private float _currPay;
    private int _empAge;
    // Constructors.
    public Employee() { }
    public Employee(string name, int id, float pay)
        :this(name, 0, id, pay){}
    public Employee(string name, int age, int id, float pay)
    {
        Name = name;
        Age = age;
        ID = id;
        Pay = pay;
    }
    // Methods.
    public void GiveBonus(float amount) => Pay += amount;

    public void DisplayStats()
    {
        Console.WriteLine("Name: {0}", Name);
        Console.WriteLine("ID: {0}", Id);
        Console.WriteLine("Age: {0}", Age);
        Console.WriteLine("Pay: {0}", Pay);
    }

    // Properties as before...
    ...
}
```

## Properties Read-Only Properties

When encapsulating data, you might want to configure a read-only property. To do so, simply omit the set block. For example, assume you have a new property named SocialSecurityNumber, which encapsulates a private string variable named empSSN. If you want to make this a read-only property, you could write this:

```
public string SocialSecurityNumber
```
