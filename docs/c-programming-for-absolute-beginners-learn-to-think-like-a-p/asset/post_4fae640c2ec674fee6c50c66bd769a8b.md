
```csharp
{
    product *= number;
}
} while (number != 0);

// Output
Console.WriteLine("Product of entered numbers (excluding zero) is: " +
product.ToString("N0"));

// Waiting for Enter
Console.ReadLine();
}
```

## Discussion

Note the following:

- The product variable starts at a value of one contrary to zero, which you used when calculating the sum.
- When updating the product, you need to take care not to include the final zero.
- You declare the product variable in type `double` for the result not to overflow. When you multiply, you quickly get big numbers.

## The Greatest

Another typical task when processing a large amount of data is searching for the extremes, in other words, the maximum or the minimum.

### Task

In this program, the user enters ten numbers. The program then outputs which one is the greatest (see Figure 24-3).

![](img/32b055c352c70d22a656320349173d95_347_0.png)

*Figure 24-3.* Outputting the greatest number

### Solution

You are going to accumulate the intermediate result again. This time, it will be the greatest number “so far.” You have to take special care with the first value; the greatest variable is set to the least possible value at the beginning in order to ascertain that the first entered number is always greater.

Because you expect exactly ten values in the input, it is more convenient to use the `for` loop here.

Here’s the code:

```csharp
static void Main(string[] args)
{
    // Preparation
    int greatest = int.MinValue;

    // Input of ten numbers
    for (int order = 1; order <= 10; order++)
```

## The Second Greatest

![](img/32b055c352c70d22a656320349173d95_349_0.png)

*Figure 24-4. Displaying the second greatest number*

### Solution

You need to remember and continuously update the two greatest numbers. It would not be enough to remember just the second greatest.

The situation resembles a ski competition, with the competitors arriving to the finish line one after another. At a certain moment, someone is the first; moments later, someone else pushes the first finisher out to the second place. Possibly in a later time, that skier may lose even the second place, simply because someone else will be better than them, or even better than the new leader.

Here’s the code:

```csharp
static void Main(string[] args)
{
    // Preparation
    int greatest = int.MinValue;
    int secondGreatest = int.MinValue;

    // Input of ten numbers
    for (int order = 1; order <= 10; order++)
    {
        // Input
        Console.Write("Enter " + order.ToString() + ". number: ");
        string input = Console.ReadLine();
        int number = Convert.ToInt32(input);

        // Is it greater than the greatest so far?
        if (number > greatest)
        {
            // Moving so far greatest to the second place
            secondGreatest = greatest;

            // Entered number becomes the greatest so far
            greatest = number;
        }
        else
        {
            // We did not beat the greatest, will we beat the second
            // greatest at least?
            if (number > secondGreatest)
            {
                secondGreatest = number;
            }
        }
    }

    // Output
    Console.WriteLine("The second greatest of entered numbers was: " +
    secondGreatest.ToString());

    // Waiting for Enter
    Console.ReadLine();
}
```

```csharp
string inReversedOrder = "";
bool terminating;

// Repeating until blank input
do
{
    // Input
    Console.Write("Enter person: ");
    string person = Console.ReadLine();

    // Processing input
    terminating = person.Trim() == "";
    if (!terminating)
    {
        inSameOrder = inSameOrder + person + ", ";
        inReversedOrder = person + ", " + inReversedOrder;
    }
} while (!terminating);

// Removing trailing comma and space
if (inSameOrder.EndsWith(", "))
{
    int numberOfCharacters = inSameOrder.Length;
    inSameOrder = inSameOrder.Remove(numberOfCharacters - 2);
}
if (inReversedOrder.EndsWith(", "))
{
    int numberOfCharacters = inReversedOrder.Length;
    inReversedOrder = inReversedOrder.Remove(numberOfCharacters - 2);
}

// Output
Console.WriteLine("Entered persons: " + inSameOrder);
Console.WriteLine("In reversed order: " + inReversedOrder);

// Waiting for Enter
Console.ReadLine();
}
```

## Discussion

Note the following:

- You use the `Trim` method to cut off possible leading or trailing spaces of the entered text in order to allow termination with any blank input, including several spaces.
- At the end, you have to get rid of the last two characters in both accumulated pieces of text. Before doing it, you test whether these two characters (a comma and a space) appear at the end of the text at all. These characters will not be there if the user immediately terminates the program by entering a blank line.
- To test whether text ends with something, you use the `EndsWith` method.

## Summary

One of the most frequent usages of loops is processing large amounts of data, be it numbers, text, or whole objects. In the loop’s body, you process a single piece of data, while the loop ascertains that all the data gets its turn.

You practiced processing larger amounts of data with examples of summing, multiplying, and finding extremes.

The most difficult exercise was that of finding the second greatest number, which required careful thinking about the possible situation that may arise depending on the data.

The last task showed you several methods to cope with text: `Trim`, `EndsWith`, and `Remove`.

### Solution

Here’s the code:

```csharp
static void Main(string[] args)
{
    // Today's date
    DateTime today = DateTime.Today;

    // Moving day after day until hit on Friday
    DateTime date = today;
    while (date.DayOfWeek != DayOfWeek.Friday)
    {
        date = date.AddDays(1);
    }

    // Calculating remaining days
    TimeSpan dateDifference = date - today;
    int daysRemaining = dateDifference.Days;

    // Outputs
    Console.WriteLine("Nearest Friday: " + date.ToShortDateString());
    Console.WriteLine("Remaining days: " + daysRemaining.ToString());
    if (daysRemaining == 0)
    {
        Console.WriteLine("Thanks God!");
    }

    // Waiting for Enter
    Console.ReadLine();
}
```

Just to remind you, $2^{10} = 2\times2\times2\times2\times2\times2\times2\times2\times2\times2 = 1024$, which is the number 2 repeated 10 times in the final product.

### Solution

The task can be solved using repeated multiplication by $x$. This means you use the intermediate result accumulation approach that you have already learned.

In principle, the solution is quite close to the one in the “Product of the Entered Numbers” section in Chapter 24:

```csharp
static void Main(string[] args)
{
    // Inputs
    Console.Write("Enter x (number to be raised): ");
    string inputX = Console.ReadLine();
    double x = Convert.ToDouble(inputX);

    Console.Write("Enter n (power): ");
    string inputN = Console.ReadLine();
    int n = Convert.ToInt32(inputN);

    // Calculating
    double result = 1;
    for (int count = 0; count < n; count++)
    {
        result *= x;
    }

    // Output
    Console.WriteLine("x^n=" + result.ToString());

    // Waiting for Enter
    Console.ReadLine();
}
```

## Sine

Continuing with mathematics, do you know how a computer actually performs calculations, for example, the sine function? If you are into mathematics, you might be interested in it.

To perform the task, you can use a so-called Taylor expansion. Some smart people found that the value of the sine function at a given point *x* (x is an angle in radians) can be calculated as a sum of infinite series:

$$\sin x = x - \frac{x^3}{3!} + \frac{x^5}{5!} - \frac{x^7}{7!} + \cdots$$

### Task

The task now is to write a program that calculates the sum of this series and compares the result to the value of the ready-made method `Math.Sin` (see Figure 25-3).

![](img/32b055c352c70d22a656320349173d95_358_0.png)

*Figure 25-3.* Calculating sine

If you are not interested in how to calculate the value of the sine function, use this task as a challenge to write a more difficult loop.

### Analysis

First, you have to analyze the calculation.
