# Solution

Here is the code:

```
```csharp
static void Main(string[] args)
{
    // Preparation - variable to accumulate intermediate result
    int sum = 0;

    // Input - 1. number
    Console.Write("Enter first number: ");
    string input = Console.ReadLine();
    int number = Convert.ToInt32(input);

    // Adding first number to intermediate result
    sum += number;

    // Input - 2. number
    Console.Write("Enter second number: ");
    input = Console.ReadLine();
    number = Convert.ToInt32(input);

    // Adding second number to intermediate result
    sum += number;

    // Input - 3. number
    Console.Write("Enter third number: ");
    input = Console.ReadLine();
    number = Convert.ToInt32(input);

    // Adding third number to intermediate result
    sum += number;

    // Output
    Console.WriteLine();
    Console.WriteLine("Sum of entered numbers: " + sum);

    // Waiting for Enter
    Console.ReadLine();
}
```
```