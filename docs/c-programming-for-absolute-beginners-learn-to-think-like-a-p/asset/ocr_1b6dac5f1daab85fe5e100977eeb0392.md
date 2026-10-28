![](bbox=[0.103, 0.109, 0.862, 0.344])

*Figure 23-1.* Repeatedly asking a question

# Solution

Here’s the code:

```
```csharp
static void Main(string[] args)
{
    string correctPassword = "friend";

    bool ok; // the variable must be declared outside of the loop!
    do
    {
        // Input
        Console.Write("Enter password: ");
        string enteredPassword = Console.ReadLine();

        // Evaluating
        ok = enteredPassword == correctPassword;
    } while (!ok); // loop repeats when the condition holds

    Console.WriteLine("Come inside, please...");

    // Waiting for Enter
    Console.ReadLine();
}
```
```