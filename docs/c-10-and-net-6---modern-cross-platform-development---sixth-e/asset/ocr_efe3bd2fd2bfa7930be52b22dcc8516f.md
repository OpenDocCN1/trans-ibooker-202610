4. Run the code and view the result, as shown in the following output:

```
aliceInPerson AS an Employee
```

What if you want to execute a block of statements when Alice is not an employee?

In the past, you would have had to use the ! (not) operator, as shown in the following code:

```
if (!(aliceInPerson is Employee))
```

With C# 9 and later, you can use the not keyword, as shown in the following code:

```
if (aliceInPerson is not Employee)
```

> Good Practice: Use the is and as keywords to avoid throwing exceptions when casting between derived types. If you don't do this, you must write try-catch statements for InvalidCastException.

# Inheriting and extending .NET types

.NET has prebuilt class libraries containing hundreds of thousands of types. Rather than creating your own completely new types, you can often get a head start by deriving from one of Microsoft's types to inherit some or all of its behavior and then overriding or extending it.

# Inheriting exceptions

As an example of inheritance, we will derive a new type of exception:

+   1. In the PacktLibrary project, add a new class file named PersonException.cs.
+   2. Modify the contents of the file to define a class named PersonException with three constructors, as shown in the following code:

```
namespace Packt.Shared;

public class PersonException : Exception
{
    public PersonException() : base() { }

    public PersonException(string message) : base(message) { }

    public PersonException(string message, Exception innerException)
        : base(message, innerException) { }
}
```