# Solution

Here is the code:

```
static void Main(string[] args)
{
    // Hinting user what we want from her
    Console.Write("Enter a sentence (and press Enter): ");

    // Reading line of text
    string input = Console.ReadLine();

    // Repeating to the output
    Console.WriteLine("You have entered: " + input);

    // Waiting for Enter
    Console.ReadLine();
}
```

# Discussion

Console.Write does not transfer the cursor to the next line, contrary to Console.WriteLine, which you have been using exclusively up to now.

# Numeric Input

In previous exercises, you were engaged with the input of text information from the user. Now you will switch to numbers, which are equally important.

# Task

You will write a program that takes a number from the user, stores it in a numeric variable, and finally repeats it to the user (see Figure 8-3).