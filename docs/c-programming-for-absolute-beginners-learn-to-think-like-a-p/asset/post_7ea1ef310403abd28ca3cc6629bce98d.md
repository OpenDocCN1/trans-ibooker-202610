
pays. What is important is that the percents are calculated from the price without VAT, not from the customer price (see Figure 10-9)!

![](img/32b055c352c70d22a656320349173d95_158_0.png)

*Figure 10-9. Understanding how the VAT works*

If the VAT rate is, for example, 21 percent, you need to divide the customer price by 1.21 to get the price without the VAT. For the general value of the tax rate, you calculate the divisor by adding the appropriate fraction to 1.

## Solution

Here is the code:

```csharp
static void Main(string[] args)
{
    // Inputs
    Console.Write("Enter customer price of a product: ");
    string inputPrice = Console.ReadLine();
    double customerPrice = Convert.ToDouble(inputPrice);

    Console.Write("Enter VAT rate in %: ");
    string inputVatRate = Console.ReadLine();
    double vatRate = Convert.ToDouble(inputVatRate);

    // Calculations
    double divisor = 1 + vatRate / 100.0;
    double calculatedPriceWithoutVat = customerPrice / divisor;
    double priceWithoutVat = Math.Round(calculatedPriceWithoutVat, 2);
    double vat = customerPrice - priceWithoutVat;

    // Outputs
    Console.WriteLine();
    Console.WriteLine("Price without VAT: " + priceWithoutVat.ToString("N2"));
    Console.WriteLine("VAT: " + vat.ToString("N2"));

    // Waiting for Enter
    Console.ReadLine();
}
```

## Summary

In this chapter, you practiced calculations on a variety of real examples from the economic world. What is always the most important in calculations like these is to understand the real-world problem first. To understand how you would get the results without a program, start with a pencil, paper, and a calculator. It is also often helpful to structure your program appropriately, dividing the whole calculation into small pieces, and to use descriptive names for your variables.

Among other things, you learned how to do rounding. Specifically, you studied several built-in mathematical functions:

- You know how to use `Math.Round` for the most common rounding, in other words, to the nearest whole number. You can specify the number of required decimal places in the second parameter of the method call.
- You know how to use `Math.Floor` for always rounding down, in other words, to the greatest integer that is less than or equal to the number being rounded.
- You know how to use `Math.Ceiling` for always rounding up, in other words, to the lowest integer that is greater than or equal to the number being rounded.

You also learned a trick of how to round to hundreds, including dividing by 100 before rounding and multiplying by the same amount afterward.

## Chapter 11: Calculations with Dates

In the previous chapter, you practiced calculations from the economic world. You will frequently need to do calculations with dates, too. Say you need to set the date when an invoice is due. Or say you want to calculate how many days an invoice is past due. Or, you might need to know the first and last days of a specific period like a month or a quarter. In this chapter, you’ll learn how to do calculations with dates.

### Date Input

First, you will learn how to read a date from a user. I will show you some simple date arithmetic, too.

#### Task

In this task, you will get a DateTime object based on the user input. After that, you will calculate the next and previous days (see Figure 11-1).

![](img/32b055c352c70d22a656320349173d95_160_0.png)

*Figure 11-1. Calculating the next and previous days*

#### Solution

The point is to use the Convert.ToDateTime method. If the user enters a nonexistent day (February 29 of a nonleap year, for example), the method causes a runtime error that you can deal with using the try-catch construction.

```csharp
static void Main(string[] args)
{
    try
    {
        // Text input of date
        Console.Write("Enter date: ");
        string input = Console.ReadLine();

        // Conversion to DateTime object
        DateTime enteredDate = Convert.ToDateTime(input);

        // Some calculations
        DateTime followingDay = enteredDate.AddDays(1);
        DateTime previousDay = enteredDate.AddDays(-1);

        // Outputs
        Console.WriteLine();
        Console.WriteLine("Entered day  : " + enteredDate.ToLongDateString());
        Console.WriteLine("Following day: " + followingDay.ToLongDateString());
        Console.WriteLine("Previous day : " + previousDay.ToLongDateString());
    }
    catch (Exception)
    {
        // Treating incorrect input
        Console.WriteLine("Incorrect input");
    }

    // Waiting for Enter
    Console.ReadLine();
}
```

#### Discussion

You can also call the Convert.ToDateTime method with two parameters instead of one. The second parameter is the language setting, which is the CultureInfo object you already know. This is similar to other conversion methods.

### Single Month

Now you will practice working with DateTime components and creating this object using a constructor call.

#### Task

A user enters a date. This program displays the first and last days of the month in which the entered date falls (see Figure 11-2).

![](img/32b055c352c70d22a656320349173d95_162_0.png)

*Figure 11-2. Calculating the first and last days of the month*

#### Solution

Here is the code:

```csharp
static void Main(string[] args)
{
    // Date input
    Console.Write("Enter a date: ");
    string input = Console.ReadLine();
    DateTime enteredDate = Convert.ToDateTime(input);

    // Calculations
    int enteredYear = enteredDate.Year;
    int enteredMonth = enteredDate.Month;

    DateTime firstDayOfMonth = new DateTime(enteredYear, enteredMonth, 1);
    DateTime lastDayOfMonth = firstDayOfMonth.AddMonths(1).AddDays(-1);

    // Outputs
    Console.WriteLine();
    Console.WriteLine("Corresponding month: " +
        "from " + firstDayOfMonth.ToShortDateString() +
        " to " + lastDayOfMonth.ToShortDateString());

    // Waiting for Enter
    Console.ReadLine();
}
```

#### Discussion

Note the following:

- According to the previous exercise, you get a `DateTime` object from the user using the `Convert.ToDateTime` method call.
- You start picking the month and year numbers from the entered date. You use the `Month` and `Year` properties for that purpose.
- Using these numbers, you easily assemble the first day of the month because its day number is always one.
- The last day of the month is not that easy because months differ in length. The trick is to add a month and subtract a day!
- Note that I do not store `AddMonths`’s result anywhere. I directly call `AddDays` upon it instead. This is called *method chaining*.
- For the sake of simplicity, I do not deal with the possibility of incorrect input here.

### Quarter

Continuing with dates, I will show you some interesting tricks you must sometimes employ to get the correct results.

#### Task

For the entered day, this program will display the beginning, end, and number (from one to four) of the year’s quarter that the day belongs to (see Figures 11-3 and 11-4).

![](img/32b055c352c70d22a656320349173d95_164_0.png)

*Figure 11-3. Showing the corresponding quarter*

![](img/32b055c352c70d22a656320349173d95_164_1.png)

*Figure 11-4. Showing the corresponding quarter, another example*

#### Analysis

The key to this task is to determine the quarter’s number. From that, the quarter’s first month follows.

#### Quarter’s Number

You need to transform the month number into the quarter’s number like this:

- Month 1, 2, or 3 = 1
- Month 4, 5, or 6 = 2
- Month 7, 8, or 9 = 3
- Month 10, 11, or 12 = 4

This is a beautiful case of integer division use. You can see that you need to add two to the month number first and perform integer division by three after that:

```csharp
int numberOfQuarter = (enteredMonth + 2) / 3;
```

#### Quarter’s First Month Number

If you already have the quarter’s number, you get the quarter’s first month like this:

- 1 (January) for the first quarter
- 4 (April) for the second quarter
- 7 (July) for the third quarter
- 10 (October) for the fourth quarter

You may realize that the quarter’s number must be multiplied by three. To get the correct results, you need to subtract two subsequently:

```csharp
int monthOfQuarterStart = 3 * numberOfQuarter - 2;
```

#### First and Last Days

Having the first month available, you can proceed in steps similar to the previous exercise. To get the first day, you use the DateTime constructor with the day number set to one. To get the last day, you add three months and subtract one day.
