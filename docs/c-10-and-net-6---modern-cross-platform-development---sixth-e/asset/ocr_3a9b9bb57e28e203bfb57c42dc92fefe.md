```
public ImmutableAnimal(string name, string species)
{
    Name = name;
    Species = species;
}

public void Deconstruct(out string name, out string species)
{
    name = Name;
    species = Species;
}
}
```

The properties, constructor, and deconstructor can be generated for you:

+   1. In Records.cs, add statements to define another record using simplified syntax known as positional records, as shown in the following code:
+   2. In Program.cs, add statements to construct and deconstruct immutable animals, as shown in the following code:
+   3. Run the application and view the results, as shown in the following output:

```
// simpler way to define a record
// auto-generates the properties, constructor, and deconstructor
public record ImmutableAnimal(string Name, string Species);
```

```
ImmutableAnimal oscar = new("Oscar", "Labrador");
var (who, what) = oscar; // calls Deconstruct method
WriteLine($"{who} is a {what}.");
```

```
Oscar is a Labrador.
```

![](bbox=[0.11, 0.67, 0.2, 0.75])

You will see records again when we look at C# 10 support for creating struct records in Chapter 6, Implementing Interfaces and Inheriting Classes.

# Practicing and exploring

Test your knowledge and understanding by answering some questions, get some hands-on practice, and explore this chapter's topics with deeper research.