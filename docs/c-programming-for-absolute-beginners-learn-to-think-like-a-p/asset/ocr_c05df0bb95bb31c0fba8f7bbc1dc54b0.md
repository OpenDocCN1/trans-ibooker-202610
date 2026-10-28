![](bbox=[0.102, 0.109, 0.86, 0.282])

# Solution

Here is the code:

```
```csharp
static void Main(string[] args)
{
    // Some text to try things on
    string text = "This is the last day of our acquaintance";

    // What e.g. can be done with texts
    Console.WriteLine("Original text: " + text);
    Console.WriteLine("Number of characters: " + text.Length);
    Console.WriteLine("In uppercase: " + text.ToUpper());
    Console.WriteLine("Does it contain word \"last\"? " + text.
    Contains("last"));

    // Waiting for Enter
    Console.ReadLine();
}
```
```

# Discussion

Data members (e.g., Length) are not accompanied by parentheses, contrary to methods (such as ToUpper, Contains), which always need parentheses even if there is nothing between them.