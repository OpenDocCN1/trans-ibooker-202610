![](bbox=[0.18, 0.07, 0.93, 0.32])

Figure 5.2: Examples of constants

![](bbox=[0.17, 0.39, 0.22, 0.44])

> Good Practice: Constants are not always the best choice for two important reasons: the value must be known at compile time, and it must be expressible as a literal string, Boolean, or number value. Every reference to the const field is replaced with the literal value at compile time, which will, therefore, not be reflected if the value changes in a future version and you do not recompile any assemblies that reference it to get the new value.

# Making a field read-only

Often a better choice for fields that should not change is to mark them as read-only:

+   1. In Person.cs, add a statement to declare an instance read-only field to store a person's home planet, as shown in the following code:
+   2. In Program.cs, add a statement to write Bob's name and home planet to the console, as shown in the following code:
+   3. Run the code and view the result, as shown in the following output:

```
// read-only fields
public readonly string HomePlanet = "Earth";
```

```
WriteLine($"{bob.Name} was born on {bob.HomePlanet}");
```

Bob Smith was born on Earth