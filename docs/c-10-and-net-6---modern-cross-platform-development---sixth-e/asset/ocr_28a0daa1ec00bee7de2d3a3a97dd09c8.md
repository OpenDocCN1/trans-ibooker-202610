Local functions can be defined anywhere inside a method: the top, the bottom, or even somewhere in the middle!

We will use a local function to implement a factorial calculation:

+   1. In Person.cs, add statements to define a Factorial function that uses a local function inside itself to calculate the result, as shown in the following code:
+   2. In Program.cs, add a statement to call the Factorial function and write the return value to the console, as shown in the following code:
+   3. Run the code and view the result, as shown in the following output:

```
// method with a local function
public static int Factorial(int number)
{
    if (number < 0)
    {
        throw new ArgumentException(
            $"{nameof(number)} cannot be less than zero.");
    }
    return localFactorial(number);

    int localFactorial(int localNumber) // local function
    {
        if (localNumber < 1) return 1;
        return localNumber * localFactorial(localNumber - 1);
    }
}
```

```
WriteLine($"5! is {Person.Factorial(5)}");
```

```
5! is 120
```

# Raising and handling events

Methods are often described as actions that an object can perform, either on itself or on related objects. For example, List<T> can add an item to itself or clear itself, and File can create or delete a file in the filesystem.

Events are often described as actions that happen to an object. For example, in a user interface, Button has a Click event, a click being something that happens to a button, and FileSystemWatcher listens to the filesystem for change notifications and raises events like Created and Deleted that are triggered when a directory or file changes.

Another way of thinking of events is that they provide a way of exchanging messages between two objects.