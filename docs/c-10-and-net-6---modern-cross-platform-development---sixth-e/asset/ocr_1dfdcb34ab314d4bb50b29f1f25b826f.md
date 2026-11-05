```
Person baby = new()
{
    Name = $"Baby of {p1.Name} and {p2.Name}"
};

p1.Children.Add(baby);
p2.Children.Add(baby);

return baby;
}

// instance method to "multiply"
public Person ProcreateWith(Person partner)
{
    return Procreate(this, partner);
}
```

Note the following:

+   - In the static method named Procreate, the Person objects to procreate are passed as parameters named p1 and p2.
+   - A new Person class named baby is created with a name composed of a combination of the two people who have procreated. This could be changed later by setting the returned baby variable's Name property.
+   - The baby object is added to the Children collection of both parents and then returned. Classes are reference types, meaning a reference to the baby object stored in memory is added, not a clone of the baby object. You will learn the difference between reference types and value types later in this chapter.
+   - In the instance method named ProcreateWith, the Person object to procreate with is passed as a parameter named partner, and it, along with this, is passed to the static Procreate method to reuse the method implementation. this is a keyword that references the current instance of the class.

![](bbox=[0.18, 0.72, 0.25, 0.78])

**Good Practice:** A method that creates a new object, or modifies an existing object, should return a reference to that object so that the caller can access the results.

2. In the PeopleApp project, at the top of the Program.cs file, delete the comment and import the namespace for our Person class and statically import the Console type, as shown in the following code:

```
using Packt.Shared;
using static System.Console;
```