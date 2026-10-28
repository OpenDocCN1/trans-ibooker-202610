```
```csharp
// Another variable (initialized with some value)
string anotherMessage = "I can't live without you.";

// Output of variables
Console.WriteLine(message);
Console.WriteLine(anotherMessage);

// Waiting for Enter
Console.ReadLine();
}
```
```

# Discussion

Now let’s discuss the solution.

# Variable Declaration

If you want to use a variable, you need to *declare* (create) it first.

The general syntax of a variable declaration statement is as follows:

*typeName*`[space]`*variableName*`[semicolon]`.

In this case, it reads as follows:

```
```csharp
string message;
```
```

The type denotes the *category of values* that you want to store in the variable. In this case, you want to store text, which is why you used the type called `string`.

# Alternative

There is an alternative way to write a variable declaration statement. In front of the semicolon, you can use an equal sign and the initial value of the variable.

Here is an example of this syntax:

```
```csharp
string anotherMessage = "I can't live without you.";
```
```