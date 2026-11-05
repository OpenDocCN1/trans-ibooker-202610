# Comparing objects when sorting

One of the most common interfaces that you will want to implement is IComparable. It has one method named CompareTo. It has two variations, one that works with a nullable object type and one that works with a nullable generic type T, as shown in the following code:

```
namespace System
{
    public interface IComparable
    {
        int CompareTo(object? obj);
    }

    public interface IComparable<in T>
    {
        int CompareTo(T? other);
    }
}
```

For example, the string type implements IComparable by returning -1 if the string is less than the string being compared to or 1 if it is greater. The int type implements IComparable by returning -1 if the int is less than the int being compared to or 1 if it is greater.

If a type implements one of the IComparable interfaces, then arrays and collections can sort it.

Before we implement the IComparable interface and its CompareTo method for the Person class, let's see what happens when we try to sort an array of Person instances:

+   1. In Program.cs, add statements that create an array of Person instances and write the items to the console, and then attempt to sort the array and write the items to the console again, as shown in the following code:

```
Person[] people =
{
    new() { Name = "Simon" },
    new() { Name = "Jenny" },
    new() { Name = "Adam" },
    new() { Name = "Richard" }
};

WriteLine("Initial list of people:");
foreach (Person p in people)
{
    WriteLine($" {p.Name}");
}

WriteLine("Use Person's IComparable implementation to sort:");
Array.Sort(people);
```