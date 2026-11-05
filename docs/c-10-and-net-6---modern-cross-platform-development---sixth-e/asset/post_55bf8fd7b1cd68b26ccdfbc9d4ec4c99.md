
```
}
catch (Exception ex)
{
    WriteLine($"{ex.GetType()} says {ex.Message}");
}
```

2. Run the code, enter Kermit again, and view the result, as shown in the following output:

```
Before parsing
What is your age? Kermit
The age you entered is not a valid number format.
After parsing
```

The reason we want to leave the more general catch below is that there might be other types of exceptions that can occur.

3. Run the code, enter 9876543210, and view the result, as shown in the following output:

```
Before parsing
What is your age? 9876543210
System.OverflowException says Value was either too large or too small for an Int32.
After parsing
```

Let's add another catch block for this type of exception.

4. Leave the existing catch blocks, and add a new catch block for the overflow exception type, as shown in the following highlighted code:

```
catch (OverflowException)
{
    WriteLine("Your age is a valid number format but it is either too big or small.");
}
catch (FormatException)
{
    WriteLine("The age you entered is not a valid number format.");
}
```

5. Run the code, enter 9876543210, and view the result, as shown in the following output:

```
Before parsing
What is your age? 9876543210
Your age is a valid number format but it is either too big or small.
After parsing
```

The order in which you catch exceptions is important. The correct order is related to the inheritance hierarchy of the exception types. You will learn about inheritance in Chapter 5, Building Your Own Types with Object-Oriented Programming. However, don't worry too much about this—the compiler will give you build errors if you get exceptions in the wrong order anyway.

### Good Practice: Avoid over-catching exceptions
Avoid over-catching exceptions. They should often be allowed to propagate up the call stack to be handled at a level where more information is known about the circumstances that could change the logic of how they should be handled. You will learn about this in *Chapter 4, Writing, Debugging, and Testing Functions.*

## Catching with filters

You can also add filters to a catch statement using the when keyword, as shown in the following code:

```
Write("Enter an amount: ");
string? amount = ReadLine();
try
{
    decimal amountValue = decimal.Parse(amount);
}
catch (FormatException) when (amount.Contains("$"))
{
    WriteLine("Amounts cannot use the dollar sign!");
}
catch (FormatException)
{
    WriteLine("Amounts must only contain digits!");
}
```

## Checking for overflow

Earlier, we saw that when casting between number types, it was possible to lose information, for example, when casting from a long variable to an int variable. If the value stored in a type is too big, it will overflow.

## Throwing overflow exceptions with the checked statement

The checked statement tells .NET to throw an exception when an overflow happens instead of allowing it to happen silently, which is done by default for performance reasons.

We will set the initial value of an int variable to its maximum value minus one. Then, we will increment it several times, outputting its value each time. Once it gets above its maximum value, it overflows to its minimum value and continues incrementing from there. Let's see this in action:

- 1. Use your preferred coding tool to add a new **Console Application** to the Chapter03 workspace/solution named CheckingForOverflow.
- 2. In Visual Studio Code, select CheckingForOverflow as the active OmniSharp project.
- 3. In Program.cs, type statements to declare and assign an integer to one less than its maximum possible value, and then increment it and write its value to the console three times, as shown in the following code:

```
int x = int.MaxValue - 1;
WriteLine($"Initial value: {x}");
x++;
WriteLine($"After incrementing: {x}");
x++;
WriteLine($"After incrementing: {x}");
x++;
WriteLine($"After incrementing: {x}");
```

- 4. Run the code and view the result that shows the value overflowing silently and wrapping around to large negative values, as shown in the following output:

```
Initial value: 2147483646
After incrementing: 2147483647
After incrementing: -2147483648
After incrementing: -2147483647
```

- 5. Now, let's get the compiler to warn us about the overflow by wrapping the statements using a checked statement block, as shown highlighted in the following code:

```
checked
{
    int x = int.MaxValue - 1;
    WriteLine($"Initial value: {x}");
    x++;
    WriteLine($"After incrementing: {x}");
    x++;
    WriteLine($"After incrementing: {x}");
    x++;
    WriteLine($"After incrementing: {x}");
}
```

- 6. Run the code and view the result that shows the overflow being checked and causing an exception to be thrown, as shown in the following output:

```
Initial value: 2147483646
After incrementing: 2147483647
Unhandled Exception: System.OverflowException: Arithmetic operation resulted in an overflow.
```

- 7. Just like any other exception, we should wrap these statements in a try statement block and display a nicer error message for the user, as shown in the following code:

```
try
{
    // previous code goes here
}
catch (OverflowException)
{
    WriteLine("The code overflowed but I caught the exception.");
}
```

- 8. Run the code and view the result, as shown in the following output:

```
Initial value: 2147483646
After incrementing: 2147483647
The code overflowed but I caught the exception.
```

## Disabling compiler overflow checks with the unchecked statement

The previous section was about the default overflow behavior at runtime and how to use the checked statement to change that behavior. This section is about compile time overflow behavior and how to use the unchecked statement to change that behavior.

A related keyword is unchecked. This keyword switches off overflow checks performed by the compiler within a block of code. Let's see how to do this:

- 1. Type the following statement at the end of the previous statements. The compiler will not compile this statement because it knows it would overflow:

```
int y = int.MaxValue + 1;
```

- 2. Hover your mouse pointer over the error, and note a compile-time check is shown as an error message, as shown in Figure 3.1:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_157_0.png)

Figure 3.1: A compile-time check in the PROBLEMS window

- 3. To disable compile-time checks, wrap the statement in an unchecked block, write the value of y to the console, decrement it, and repeat, as shown in the following code:

```
unchecked
{
    int y = int.MaxValue + 1;
    WriteLine($"Initial value: {y}");
    y--;
    WriteLine($"After decrementing: {y}");
    y--;
    WriteLine($"After decrementing: {y}");
}
```

- 4. Run the code and view the results, as shown in the following output:

```
Initial value: -2147483648
After decrementing: 2147483647
After decrementing: 2147483646
```

Of course, it would be rare that you would want to explicitly switch off a check like this because it allows an overflow to occur. But perhaps you can think of a scenario where you might want that behavior.

## Practicing and exploring

Test your knowledge and understanding by answering some questions, get some hands-on practice, and explore with deeper research into this chapter's topics.

### Exercise 3.1 – Test your knowledge

Answer the following questions:

- 1. What happens when you divide an int variable by 0?
- 2. What happens when you divide a double variable by 0?
- 3. What happens when you overflow an int variable, that is, set it to a value beyond its range?
- 4. What is the difference between x = y++; and x = ++y;?
- 5. What is the difference between break, continue, and return when used inside a loop statement?
- 6. What are the three parts of a for statement and which of them are required?
- 7. What is the difference between the = and == operators?
- 8. Does the following statement compile?

```
for ( ; true; );
```

- 9. What does the underscore _ represent in a switch expression?
- 10. What interface must an object implement to be enumerated over by using the foreach statement?

### Exercise 3.2 – Explore loops and overflow

What will happen if this code executes?

```
int max = 500;
for (byte i = 0; i < max; i++)
{
    WriteLine(i);
}
```

Create a console application in Chapter03 named Exercise02 and enter the preceding code. Run the console application and view the output. What happens?

What code could you add (don't change any of the preceding code) to warn us about the problem?

### Exercise 3.3 – Practice loops and operators

FizzBuzz is a group word game for children to teach them about division. Players take turns to count incrementally, replacing any number divisible by three with the word fizz, any number divisible by five with the word buzz, and any number divisible by both with fizzbuzz.

Create a console application in Chapter03 named Exercise03 that outputs a simulated FizzBuzz game counting up to 100. The output should look something like Figure 3.2:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_159_0.png)

Figure 3.2: A simulated FizzBuzz game output
