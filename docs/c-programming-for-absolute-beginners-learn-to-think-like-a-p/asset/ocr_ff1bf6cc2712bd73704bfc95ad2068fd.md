# ## Solution

Here is the solution:

```
```csharp
static void Main(string[] args)
{
    // Prompting the user
    Console.Write("Enter year of your birth: ");

    // Reading line of text
    string input = Console.ReadLine();

    // CONVERTING TO NUMBER (of entered text)
    int yearOfBirth = Convert.ToInt32(input);

    // Finding this year
    DateTime today = DateTime.Today;
    int thisYear = today.Year;

    // Calculating age
    int age = thisYear - yearOfBirth;

    // Outputting the result
    Console.WriteLine("This year you are/will be: " + age);

    // Waiting for Enter
    Console.ReadLine();
}
```
```

# ## Ten More

Let’s continue with the calculations.

# ## Task

You will write a program that accepts a number from the user. After that, it displays a number that is greater by ten than the one entered (see Figure 8-5).