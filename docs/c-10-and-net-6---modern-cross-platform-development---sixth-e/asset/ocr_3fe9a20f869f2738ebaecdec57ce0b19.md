# Serializing object graphs

Serialization is the process of converting a live object into a sequence of bytes using a specified format. Deserialization is the reverse process. You would do this to save the current state of a live object so that you can recreate it in the future. For example, saving the current state of a game so that you can continue at the same place tomorrow. Serialized objects are usually stored in a file or database.

There are dozens of formats you can specify, but the two most common ones are eXtensible Markup Language (XML) and JavaScript Object Notation (JSON).

![](bbox=[0.1, 0.32, 0.2, 0.4])

> **Good Practice:** JSON is more compact and is best for web and mobile applications. XML is more verbose but is better supported in more legacy systems. Use JSON to minimize the size of serialized object graphs. JSON is also a good choice when sending object graphs to web applications and mobile applications because JSON is the native serialization format for JavaScript and mobile apps often make calls over limited bandwidth, so the number of bytes is important.

.NET has multiple classes that will serialize to and from XML and JSON. We will start by looking at XmlSerializer and JsonSerializer.

# Serializing as XML

Let's start by looking at XML, probably the world's most used serialization format (for now). To show a typical example, we will define a custom class to store information about a person and then create an object graph using a list of Person instances with nesting:

+   1. Use your preferred code editor to add a new console app named WorkingWithSerialization to the Chapter09 solution/workspace.
+   2. In Visual Studio Code, select WorkingWithSerialization as the active OmniSharp project.
+   3. Add a class named Person with a Salary property that is protected, meaning it is only accessible to itself and derived classes. To populate the salary, the class has a constructor with a single parameter to set the initial salary, as shown in the following code:

```
namespace Packt.Shared;

public class Person
{
    public Person(decimal initialSalary)
    {
        Salary = initialSalary;
    }
}
```