```
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