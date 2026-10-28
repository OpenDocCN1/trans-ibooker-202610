![](bbox=[0.104, 0.109, 0.86, 0.285])

**Figure 19-5.** *Incorrect label, not crashing*

# Solution

Here’s the code:

```
static void Main(string[] args)
{
    // Input
    Console.Write("Enter product label: ");
    string label = Console.ReadLine();

    // Evaluating
    if (label.Length >= 2 && label.Substring(1, 1) == "X")
    {
        Console.WriteLine("Label is OK");
    }
    else
    {
        Console.WriteLine("Incorrect label");
    }

    // Waiting for Enter
    Console.ReadLine();
}
```

# Discussion

The following sections explain the program.