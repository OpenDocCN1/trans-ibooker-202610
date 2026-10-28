# Task

The task is to write a program that reads a decimal number with two fixed-language settings, American and Czech.

# Solution

To work with specific localizations, use the `CultureInfo` object. Also, please do not forget to insert the `using System.Globalization;` line at the top of your source code.

Here is the code:

```
```csharp
static void Main(string[] args)
{
    // AMERICAN
    CultureInfo american = new CultureInfo("en-US");
    try
    {
        // Input
        Console.Write("Enter American decimal number: ");
        string input = Console.ReadLine();
        double number = Convert.ToDouble(input, american);
        // Output
        Console.WriteLine("You have entered " + number);
    }
    catch (Exception)
    {
        // Error message
        Console.WriteLine("Incorrect input");
    }

    // CZECH
    CultureInfo czech = new CultureInfo("cs-CZ");
    try
    {
        // Input
        Console.WriteLine();
        Console.Write("Enter Czech decimal number: ");
```
```