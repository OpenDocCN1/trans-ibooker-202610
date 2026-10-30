A C# property is composed by defining a get scope (accessor) and set scope (mutator) directly within the property itself. Notice that the property specifies the type of data it is encapsulating by what appears to be a return value. Also take note that, unlike a method, properties do not make use of parentheses (not even empty parentheses) when being defined. Consider the following commentary on your current Id property:

```
// The 'int' represents the type of data this property encapsulates.
public int Id // Note lack of parentheses.
{
    get { return _empId; }
    set { _empID = value; }
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