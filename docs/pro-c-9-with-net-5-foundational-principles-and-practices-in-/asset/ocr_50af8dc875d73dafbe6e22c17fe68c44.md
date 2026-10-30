> Note Convention is that protected members are named Pascal-Cased (EmpName) and not Underscore-Camel-Case (_empName). This is not a requirement of the language, but a common code style. If you decide to update the names as I have done here, make sure to rename all of the backing methods in your properties to match the Pascal-Cased protected properties.

The benefit of defining protected members in a base class is that derived types no longer have to access the data indirectly using public methods or properties. The possible downfall, of course, is that when a derived type has direct access to its parent’s internal data, it is possible to accidentally bypass existing business rules found within public properties. When you define protected members, you are creating a level of trust between the parent class and the child class, as the compiler will not catch any violation of your type’s business rules.

Finally, understand that as far as the object user is concerned, protected data is regarded as private (as the user is "outside" the family). Therefore, the following is illegal:

```
// Error! Can't access protected data from client code.
Employee emp = new Employee();
emp.empName = "Fred";
```

> Note Although protected field data can break encapsulation, it is quite safe (and useful) to define protected methods. When building class hierarchies, it is common to define a set of methods that are only for use by derived types and are not intended for use by the outside world.

# Adding a sealed Class

Recall that a sealed class cannot be extended by other classes. As mentioned, this technique is most often used when you are designing a utility class. However, when building class hierarchies, you might find that a certain branch in the inheritance chain should be "capped off," as it makes no sense to further extend the lineage. For example, assume you have added yet another class to your program (PtSalesPerson) that extends the existing SalesPerson type. Figure 6-3 shows the current update.