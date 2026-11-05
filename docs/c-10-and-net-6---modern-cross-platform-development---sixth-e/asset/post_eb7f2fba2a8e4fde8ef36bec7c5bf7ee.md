
## Exercise 3.4 – Practice exception handling

Create a console application in Chapter03 named Exercise04 that asks the user for two numbers in the range 0-255 and then divides the first number by the second:

```
Enter a number between 0 and 255: 100
Enter another number between 0 and 255: 8
100 divided by 8 is 12
```

Write exception handlers to catch any thrown errors, as shown in the following output:

```
Enter a number between 0 and 255: apples
Enter another number between 0 and 255: bananas
FormatException: Input string was not in a correct format.
```

## Exercise 3.5 – Test your knowledge of operators

What are the values of x and y after the following statements execute?

- 1. Increment and addition operators:
```
x = 3;
y = 2 + ++x;
```
- 2. Binary shift operators:
```
x = 3 << 2;
y = 10 >> 1;
```
- 3. Bitwise operators:
```
x = 10 & 8;
y = 10 | 7;
```

## Exercise 3.6 – Explore topics

Use the links on the following page to learn about the topics covered in this chapter in more detail:

https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-3---controlling-flow-and-converting-types

## Summary

In this chapter, you experimented with some operators, learned how to branch and loop, how to convert between types, and how to catch exceptions.

You are now ready to learn how to reuse blocks of code by defining functions, how to pass values into them and get values back, and how to track down bugs in your code and squash them!

## 04
### Writing, Debugging, and Testing Functions

This chapter is about writing functions to reuse code, debugging logic errors during development, logging exceptions during runtime, unit testing your code to remove bugs, and ensuring stability and reliability.

This chapter covers the following topics:

- Writing functions
- Debugging during development
- Logging during runtime
- Unit testing
- Throwing and catching exceptions in functions

### Writing functions

A fundamental principle of programming is Don't Repeat Yourself (DRY).

While programming, if you find yourself writing the same statements over and over again, then turn those statements into a function. Functions are like tiny programs that complete one small task. For example, you might write a function to calculate sales tax and then reuse that function in many places in a financial application.

Like programs, functions usually have inputs and outputs. They are sometimes described as black boxes, where you feed some raw materials in one end, and a manufactured item emerges at the other. Once created, you don't need to think about how they work.

### Times table example

Let's say that you want to help your child learn their times tables, so you want to make it easy to generate a times table for a number, such as the 12 times table:

```
1 x 12 = 12
2 x 12 = 24
...
12 x 12 = 144
```

You previously learned about the for statement earlier in this book, so you know that it can be used to generate repeated lines of output when there is a regular pattern, such as the 12 times table, as shown in the following code:

```
for (int row = 1; row <= 12; row++)
{
    Console.WriteLine($"{row} x 12 = {row * 12}");
}
```

However, instead of outputting the 12 times table, we want to make this more flexible, so it could output the times table for any number. We can do this by creating a function.

### Writing a times table function

Let's explore functions by creating one to output any times table for numbers 0 to 255 multiplied by 1 to 12:

- 1. Use your preferred coding tool to create a new console app, as defined in the following list:
  - 1. Project template: Console Application / console
  - 2. Workspace/solution file and folder: Chapter04
  - 3. Project file and folder: WritingFunctions
- 2. Statically import System.Console.
- 3. In Program.cs, write statements to define a function named TimesTable, as shown in the following code:

```
static void TimesTable(byte number)
{
    WriteLine($"This is the {number} times table:");

    for (int row = 1; row <= 12; row++)
    {
        WriteLine($"{row} x {number} = {row * number}");
    }
    WriteLine();
}
```

In the preceding code, note the following:

- TimesTable must have a byte value passed to it as a parameter named number.
- TimesTable is a static method because it will be called by the static method Main.
- TimesTable does not return a value to the caller, so it is declared with the void keyword before its name.
- TimesTable uses a for statement to output the times table for the number passed to it.

4. After the statement that statically imports the Console class and before the TimesTable function, call the function and pass in a byte value for the number parameter, for example, 6, as shown highlighted in the following code:

```
using static System.Console;

TimesTable(6);
```

Good Practice: If a function has one or more parameters where just passing the values may not provide enough meaning, then you can optionally specify the name of the parameter as well as its value, as shown in the following code: TimesTable(number: 6).

5. Run the code and then view the result, as shown in the following output:

```
This is the 6 times table:
1 x 6 = 6
2 x 6 = 12
3 x 6 = 18
4 x 6 = 24
5 x 6 = 30
6 x 6 = 36
7 x 6 = 42
8 x 6 = 48
9 x 6 = 54
10 x 6 = 60
11 x 6 = 66
12 x 6 = 72
```

6. Change the number passed into the TimesTable function to other byte values between 0 and 255 and confirm that the output times tables are correct.

7. Note that if you try to pass a non-byte number, for example, an int or double or string, an error is returned, as shown in the following output:

```
Error: (1,12): error CS1503: Argument 1: cannot convert from 'int' to 'byte'
```

### Writing a function that returns a value

The previous function performed actions (looping and writing to the console), but it did not return a value. Let's say that you need to calculate sales or value-added tax (VAT). In Europe, VAT rates can range from 8% in Switzerland to 27% in Hungary. In the United States, state sales taxes can range from 0% in Oregon to 8.25% in California.

> Tax rates change all the time, and they vary based on many factors. You do not need to contact me to tell me that the tax rate in Virginia is 6%. Thank you.

Let's implement a function to calculate taxes in various regions around the world:

- 1. Add a function named CalculateTax, as shown in the following code:

```
static decimal CalculateTax(
    decimal amount, string twoLetterRegionCode)
{
    decimal rate = 0.0M;

    switch (twoLetterRegionCode)
    {
        case "CH": // Switzerland
            rate = 0.08M;
            break;
        case "DK": // Denmark
        case "NO": // Norway
            rate = 0.25M;
            break;
        case "GB": // United Kingdom
        case "FR": // France
            rate = 0.2M;
            break;
        case "HU": // Hungary
            rate = 0.27M;
            break;
        case "OR": // Oregon
        case "AK": // Alaska
        case "MT": // Montana
            rate = 0.0M;
            break;
        case "ND": // North Dakota
        case "WI": // Wisconsin
        case "ME": // Maine
        case "VA": // Virginia
            rate = 0.05M;
            break;
        case "CA": // California
            rate = 0.0825M;
            break;
        default: // most US states
            rate = 0.06M;
            break;
    }

    return amount * rate;
}
```

In the preceding code, note the following:

- CalculateTax has two inputs: a parameter named amount that will be the amount of money spent, and a parameter named twoLetterRegionCode that will be the region the amount is spent in.
- CalculateTax will perform a calculation using a switch statement and then return the sales tax or VAT owed on the amount as a decimal value; so, before the name of the function, we have declared the data type of the return value to be decimal.

2. Comment out the TimesTable method call and call the CalculateTax method, passing values for the amount such as 149 and a valid region code such as FR, as shown in the following code:

```
// TimesTable(6);

decimal taxToPay = CalculateTax(amount: 149, twoLetterRegionCode: "FR");
WriteLine($"You must pay {taxToPay} in tax.");
```

3. Run the code and view the result, as shown in the following output:

```
You must pay 29.8 in tax.
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_165_0.png)

> We could format the taxToPay output as currency by using {taxToPay:C} but it will use your local culture to decide how to format the currency symbol and decimals. For example, for me in the UK, I would see £29.80.

Can you think of any problems with the CalculateTax function as written? What would happen if the user enters a code such as fr or UK? How could you rewrite the function to improve it? Would using a switch expression instead of a switch statement be clearer?
