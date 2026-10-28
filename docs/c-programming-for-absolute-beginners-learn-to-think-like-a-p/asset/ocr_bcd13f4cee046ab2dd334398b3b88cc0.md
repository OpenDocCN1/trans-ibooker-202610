# Solution

Here’s the code:

```
```csharp
static void Main(string[] args)
{
    // Inputs
    Console.Write("Enter text to repeat: ");
    string textToRepeat = Console.ReadLine();

    Console.Write("Enter number of repetitions: ");
    string input = Console.ReadLine();
    int howManyTimes = Convert.ToInt32(input);

    // Output
    for (int count = 0; count < howManyTimes; count++)
    {
        Console.WriteLine(textToRepeat);
    }

    // Waiting for Enter
    Console.ReadLine();
}
```
```

# Alternating Loop

Quite frequently, you need to repeat a couple of activities. You do the first thing, then the second one, and the first one again, and so on. It is interesting to look at such a task in code. I will show you three ways of how to solve this.

# Task

You will write a program that alternates between two tasks in a to-do list (see Figure 21–2).