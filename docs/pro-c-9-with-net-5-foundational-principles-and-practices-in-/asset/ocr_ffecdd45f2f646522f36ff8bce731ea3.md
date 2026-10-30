class that does not maintain any object-level state and is not created with the new keyword. Rather, a utility class exposes all functionality as class-level (aka static) members.

For example, if you were to use the Visual Studio Object Browser (via the View ▶ Object Browser menu item) to view the System namespace, you would see that all the members of the Console, Math, Environment, and GC classes (among others) expose all their functionality via static members. These are but a few utility classes found within the .NET Core base class libraries.

Again, be aware that static members are not only found in utility classes; they can be part of any class definition at all. Just remember that static members promote a given item to the class level rather than the object level. As you will see over the next few sections, the static keyword can be applied to the following:

+   - Data of a class
+   - Methods of a class
+   - Properties of a class
+   - A constructor
+   - The entire class definition
+   - In conjunction with the C# using keyword

Let’s see each of our options, beginning with the concept of static data.

> Note You will examine the role of static properties later in this chapter while examining the properties themselves.

# Defining Static Field Data

Most of the time when designing a class, you define data as instance-level data or, said another way, as nonstatic data. When you define instance-level data, you know that every time you create a new object, the object maintains its own independent copy of the data. In contrast, when you define static data of a class, the memory is shared by all objects of that category.

To see the distinction, create a new Console Application project named StaticDataAndMembers. Now, insert a file into your project named SavingsAccount.cs, and in that file create a new class named SavingsAccount. Begin by defining an instance-level variable (to model the current balance) and a custom constructor to set the initial balance.

```
using System;
namespace StaticDataAndMembers
{
    // A simple savings account class.
    class SavingsAccount
    {
        // Instance-level data.
        public double currBalance;
        public SavingsAccount(double balance)
        {
            currBalance = balance;
        }
    }
}
```