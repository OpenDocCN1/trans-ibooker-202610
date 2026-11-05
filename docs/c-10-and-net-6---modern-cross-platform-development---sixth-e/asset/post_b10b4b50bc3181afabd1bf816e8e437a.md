
## Converting numbers from cardinal to ordinal

Numbers that are used to count are called **cardinal** numbers, for example, 1, 2, and 3, whereas numbers used to order are **ordinal** numbers, for example, 1st, 2nd, and 3rd. Let's create a function to convert cardinals to ordinals:

- 1. Write a function named CardinalToOrdinal that converts a cardinal int value into an ordinal string value; for example, it converts 1 into 1st, 2 into 2nd, and so on, as shown in the following code:

```
static string CardinalToOrdinal(int number)
{
    switch (number)
    {
        case 11: // special cases for 11th to 13th
        case 12:
        case 13:
            return $"{number}th";
        default:
            int lastDigit = number % 10;

            string suffix = lastDigit switch
            {
                1 => "st",
                2 => "nd",
                3 => "rd",
                _ => "th"
            };
            return $"{number}{suffix}";
    }
}
```

From the preceding code, note the following:

- CardinalToOrdinal has one input: a parameter of the int type named number, and one output: a return value of the string type.
- A switch statement is used to handle the special cases of 11, 12, and 13.
- A switch expression then handles all other cases: if the last digit is 1, then use st as the suffix; if the last digit is 2, then use nd as the suffix; if the last digit is 3, then use rd as the suffix; and if the last digit is anything else, then use th as the suffix.

- 2. Write a function named RunCardinalToOrdinal that uses a for statement to loop from 1 to 40, calling the CardinalToOrdinal function for each number and writing the returned string to the console, separated by a space character, as shown in the following code:

```
static void RunCardinalToOrdinal()
{
    for (int number = 1; number <= 40; number++)
    {
        Write($"{CardinalToOrdinal(number)} ");
    }
    WriteLine();
}
```

3. Comment out the CalculateTax statements, and call the RunCardinalToOrdinal method, as shown in the following code:

```
// TimesTable(6);

// decimal taxToPay = CalculateTax(amount: 149, twoLetterRegionCode: "FR");
// WriteLine($"You must pay {taxToPay} in tax.");

RunCardinalToOrdinal();
```

4. Run the code and view the results, as shown in the following output:

```
1st 2nd 3rd 4th 5th 6th 7th 8th 9th 10th 11th 12th 13th 14th 15th 16th
17th 18th 19th 20th 21st 22nd 23rd 24th 25th 26th 27th 28th 29th 30th 31st
32nd 33rd 34th 35th 36th 37th 38th 39th 40th
```

## Calculating factorials with recursion

The factorial of 5 is 120, because factorials are calculated by multiplying the starting number by one less than itself, and then by one less again, and so on, until the number is reduced to 1. An example can be seen here: 5 x 4 x 3 x 2 x 1 = 120.

Factorials are written like this: 5!, where the exclamation mark is read as bang, so 5! = 120, that is, five bang equals one hundred and twenty. Bang is a good name for factorials because they increase in size very rapidly, just like an explosion.

We will write a function named Factorial; this will calculate the factorial for an int passed to it as a parameter. We will use a clever technique called recursion, which means a function that calls itself within its implementation, either directly or indirectly:

1. Add a function named Factorial, and a function to call it, as shown in the following code:

```
static int Factorial(int number)
{
    if (number < 1)
    {
        return 0;
    }
    else if (number == 1)
    {
        return 1;
    }
    else
    {
        return number * Factorial(number - 1);
    }
}
```

As before, there are several noteworthy elements of the preceding code, including the following:

- If the input parameter number is zero or negative, Factorial returns 0.
- If the input parameter number is 1, Factorial returns 1, and therefore stops calling itself.
- If the input parameter number is larger than one, which it will be in all other cases, Factorial multiplies the number by the result of calling itself and passing one less than number. This makes the function recursive.

**More Information:** Recursion is clever, but it can lead to problems, such as a stack overflow due to too many function calls because memory is used to store data on every function call, and it eventually uses too much. Iteration is a more practical, if less succinct, solution in languages such as C#. You can read more about this at the following link: https://en.wikipedia.org/wiki/Recursion_(computer_science)#Recursion_versus_iteration.

2. Add a function named RunFactorial that uses a for statement to output the factorials of numbers from 1 to 14, calls the Factorial function inside its loop, and then outputs the result, formatted using the code N0, which means number format uses thousand separators with zero decimal places, as shown in the following code:

```
static void RunFactorial()
{
    for (int i = 1; i < 15; i++)
    {
        WriteLine($"{i}! = {Factorial(i):N0}");
    }
}
```

3. Comment out the RunCardinalToOrdinal method call and call the RunFactorial method.

4. Run the code and view the results, as shown in the following output:

```
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
6! = 720
7! = 5,040
8! = 40,320
9! = 362,880
10! = 3,628,800
11! = 39,916,800
12! = 479,001,600
13! = 1,932,053,504
14! = 1,278,945,280
```

It is not immediately obvious in the previous output, but factorials of 13 and higher overflow the int type because they are so big. 12! is 479,001,600, which is about half a billion. The maximum positive value that can be stored in an int variable is about two billion. 13! is 6,227,020,800, which is about six billion and when stored in a 32-bit integer it overflows silently without showing any problems.

Do you remember what we can do to be notified of a numeric overflow?

What should you do to get notified when an overflow happens? Of course, we could solve the problem for 13! and 14! by using a long (64-bit integer) instead of an int (32-bit integer), but we will quickly hit the overflow limit again.

The point of this section is to understand that numbers can overflow and how to show that rather than ignore it, not specifically how to calculate factorials higher than 12!.

1. Modify the Factorial function to check for overflows, as shown highlighted in the following code:

```
checked // for overflow
{
    return number * Factorial(number - 1);
}
```

2. Modify the RunFactorial function to handle overflow exceptions when calling the Factorial function, as shown highlighted in the following code:

```
try
{
    WriteLine($"{i}! = {Factorial(i):N0}");
}
catch (System.OverflowException)
{
    WriteLine($"{i}! is too big for a 32-bit integer.");
}
```

3. Run the code and view the results, as shown in the following output:

```
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
6! = 720
7! = 5,040
8! = 40,320
9! = 362,880
10! = 3,628,800
11! = 39,916,800
12! = 479,001,600
13! is too big for a 32-bit integer.
14! is too big for a 32-bit integer.
```

## Documenting functions with XML comments

By default, when calling a function such as CardinalToOrdinal, code editors will show a tooltip with basic information, as shown in Figure 4.1:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_170_0.png)

Figure 4.1: A tooltip showing the default simple method signature

Let's improve the tooltip by adding extra information:

- 1. If you are using Visual Studio Code with the C# extension, you should navigate to View | Command Palette | Preferences: Open Settings (UI), and then search for formatOnType and make sure that is enabled. C# XML documentation comments are a built-in feature of Visual Studio 2022.
- 2. On the line above the CardinalToOrdinal function, type three forward slashes ///, and note that they are expanded into an XML comment that recognizes that the function has a single parameter named number.

3. Enter suitable information for the XML documentation comment for a summary and to describe the input parameter and the return value for the CardinalToOrdinal function, as shown in the following code:

```
/// <summary>
/// Pass a 32-bit integer and it will be converted into its ordinal equivalent.
/// </summary>
/// <param name="number">Number is a cardinal value e.g. 1, 2, 3, and so on.</param>
/// <returns>Number as an ordinal value e.g. 1st, 2nd, 3rd, and so on.</returns>
```

4. Now, when calling the function, you will see more details, as shown in Figure 4.2:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_171_0.png)

Figure 4.2: A tooltip showing the more detailed method signature

At the time of writing the sixth edition, C# XML documentation comments do not work in .NET Interactive notebooks.

> Good Practice: Add XML documentation comments to all your functions.

## Using lambdas in function implementations

F# is Microsoft's strongly typed functional-first programming language that, like C#, compiles to IL to be executed by .NET. Functional languages evolved from lambda calculus; a computational system based only on functions. The code looks more like mathematical functions than steps in a recipe.

Some of the important attributes of functional languages are defined in the following list:

- **Modularity**: The same benefit of defining functions in C# applies to functional languages. Break up a large complex code base into smaller pieces.
- **Immutability**: Variables in the C# sense do not exist. Any data value inside a function cannot change. Instead, a new data value can be created from an existing one. This reduces bugs.
