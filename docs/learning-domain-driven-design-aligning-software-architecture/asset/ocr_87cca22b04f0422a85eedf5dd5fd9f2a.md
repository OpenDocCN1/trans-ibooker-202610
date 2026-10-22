```
class Person
{
    public readonly PersonId Id;
    public Name Name { get; set; }

    public Person(PersonId id, Name name)
    {
        this.Id = id;
        this.Name = name;
    }
}
```

In the preceding code, we introduced the identification field Id of type PersonId. PersonId is a value object, and it can use any underlying data types that fit the business domain’s needs. For example, the Id can be a GUID, a number, a string, or a domain-specific value such as a Social Security number.

The central requirement for the identification field is that it should be unique for each instance of the entity: for each person, in our case (Figure 6-2). Furthermore, except for very rare exceptions, the value of an entity’s identification field should remain immutable throughout the entity’s lifecycle. This brings us to the second conceptual difference between value objects and entities.

![](bbox=[0.14, 0.6, 0.89, 0.83])

Figure 6-2. Introducing an explicit identification field, allowing differentiating instances of the object even if the values of all other fields are identical