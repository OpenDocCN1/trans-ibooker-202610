A student has an *Excellent* evaluation when

+   - The arithmetic average of all the grades is not higher than 1.5.
+   - The student does not have any grade worse than 2.
+   - The student does not have any unexcused absence.

The student is considered *Failed* when at least one of their grades is 5.
In all other cases, the summary evaluation is *Good*.
You can probably guess now that the program is not going to be trivial.

# Solution

Here’s the code:

```
static void Main(string[] args)
{
    // 1. PREPARATIONS
    string errorMessage = "Incorrect input";
    int mathematics, informationTechnology, science, english;
    bool hasUnexcusedAbsences;

    // 2. INPUTS
    try
    {
        Console.WriteLine("Enter grades from individual subjects:");

        Console.Write("Mathematics: ");
        string input = Console.ReadLine();
        mathematics = Convert.ToInt32(input);
        if (mathematics < 1 || mathematics > 5)
        {
            Console.WriteLine(errorMessage);
            return;
        }
```