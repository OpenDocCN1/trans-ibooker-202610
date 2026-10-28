# Solution

Here is the code:

```
static void Main(string[] args)
{
    // Date input
    Console.Write("Enter a date: ");
    string input = Console.ReadLine();
    DateTime enteredDate = Convert.ToDateTime(input);

    // Calculations
    int enteredYear = enteredDate.Year;
    int enteredMonth = enteredDate.Month;

    int numberOfQuarter = (enteredMonth + 2) / 3;
    int monthOfQuarterStart = 3 * numberOfQuarter - 2;
    DateTime firstDayOfQuarter = new DateTime(enteredYear,
    monthOfQuarterStart, 1);
    DateTime lastDayOfQuarter = firstDayOfQuarter.AddMonths(3).AddDays(-1);

    // Outputs
    Console.WriteLine();
    Console.WriteLine("Corresponding quarter: " +
        "number-" + numberOfQuarter +
        ", from " + firstDayOfQuarter.ToShortDateString() +
        " to " + lastDayOfQuarter.ToShortDateString());

    // Waiting for Enter
    Console.ReadLine();
}
```

# Date Difference

You frequently need to calculate the time span between two specific dates, in other words, how many days or years have passed between the entered dates. This is what you will study now.