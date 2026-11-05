
By default, a date and time value outputs with the short date and time format. You can use format codes such as D to output only the date part using the long date format.

![](img/c63b00dca7192cc58cfb9e1f1e64f483_150_0.png)

> Good Practice: Use the standard date and time format specifiers, as shown at the following link: https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-date-and-time-format-strings#table-of-format-specifiers

## Errors using Parse

One problem with the Parse method is that it gives errors if the string cannot be converted.

- 1. Type a statement to attempt to parse a string containing letters into an integer variable, as shown in the following code:
- 2. Run the code and view the result, as shown in the following output:

```
int count = int.Parse("abc");
```

```
Unhandled Exception: System.FormatException: Input string was not in a correct format.
```

As well as the preceding exception message, you will see a stack trace. I have not included stack traces in this book because they take up too much space.

## Avoiding exceptions using the TryParse method

To avoid errors, you can use the TryParse method instead. TryParse attempts to convert the input string and returns true if it can convert it and false if it cannot.

The out keyword is required to allow the TryParse method to set the count variable when the conversion works.

Let's see TryParse in action:

- 1. Replace the int count declaration with statements to use the TryParse method and ask the user to input a count for a number of eggs, as shown in the following code:

```
Write("How many eggs are there? ");
string? input = ReadLine(); // or use "12" in notebook

if (int.TryParse(input, out int count))
{
    WriteLine($"There are {count} eggs.");
}
else
{
    WriteLine("I could not parse the input.");
}
```

- 2. Run the code, enter 12, and view the result, as shown in the following output:

```
How many eggs are there? 12
There are 12 eggs.
```

- 3. Run the code, enter twelve (or change the string value to "twelve" in a notebook), and view the result, as shown in the following output:

```
How many eggs are there? twelve
I could not parse the input.
```

You can also use methods of the System.Convert type to convert string values into other types; however, like the Parse method, it gives an error if it cannot convert.

## Handling exceptions

You've seen several scenarios where errors have occurred when converting types. Some languages return error codes when something goes wrong. .NET uses exceptions that are richer and designed only for failure reporting compared to return values that have multiple uses. When this happens, we say a runtime exception has been thrown.

When an exception is thrown, the thread is suspended and if the calling code has defined a try-catch statement, then it is given a chance to handle the exception. If the current method does not handle it, then its calling method is given a chance, and so on up the call stack.

As you have seen, the default behavior of a console application or a .NET Interactive notebook is to output a message about the exception, including a stack trace, and then stop running the code. The application is terminated. This is better than allowing the code to continue executing in a potentially corrupt state. Your code should only catch and handle exceptions that it understands and can properly fix.

![](img/c63b00dca7192cc58cfb9e1f1e64f483_151_0.png)

> Good Practice: Avoid writing code that will throw an exception whenever possible, perhaps by performing if statement checks. Sometimes you can't, and sometimes it is best to allow the exception to be caught by a higher-level component that is calling your code. You will learn how to do this in Chapter 4, Writing, Debugging, and Testing Functions.

## Wrapping error-prone code in a try block

When you know that a statement can cause an error, you should wrap that statement in a try block. For example, parsing from text to a number can cause an error. Any statements in the catch block will be executed only if an exception is thrown by a statement in the try block.

We don't have to do anything inside the catch block. Let's see this in action:

- 1. Use your preferred coding tool to add a new Console Application to the Chapter03 workspace/solution named HandlingExceptions.
- 2. In Visual Studio Code, select HandlingExceptions as the active OmniSharp project.
- 3. Type statements to prompt the user to enter their age and then write their age to the console, as shown in the following code:

```
WriteLine("Before parsing");
Write("What is your age? ");
string? input = ReadLine(); // or use "49" in a notebook

try
{
    int age = int.Parse(input);
    WriteLine($"You are {age} years old.");
}
catch
{
}
WriteLine("After parsing");
```

> You will see the following compiler message: Warning CS8604 Possible null reference argument for parameter 's' in 'int int.Parse(string s)'. By default in new .NET 6 projects, Microsoft has enabled nullable reference types so you will see many more compiler warnings like this. In production code, you should add code to check for null and handle that possibility appropriately. In this book, I will not include these null checks because the code samples are not designed to be production quality and null checks everywhere will clutter the code and use up valuable pages. In this case, it is impossible for input to be null because the user must press Enter for ReadLine to return and that will return an empty string. You will see hundreds of more examples of potentially null variables throughout the code samples in this book. Those warnings are safe to ignore for the book code examples. You only need similar warnings when you write your own production code. You will see more about null handling in Chapter 6, Implementing Interfaces and Inheriting Classes.

This code includes two messages to indicate before parsing and after parsing to make clearer the flow through the code. These will be especially useful as the example code grows more complex.

- 4. Run the code, enter 49, and view the result, as shown in the following output:

```
Before parsing
What is your age? 49
You are 49 years old.
After parsing
```

- 5. Run the code, enter Kermit, and view the result, as shown in the following output:

```
Before parsing
What is your age? Kermit
After parsing
```

When the code was executed, the error exception was caught and the default message and stack trace were not output, and the console application continued running. This is better than the default behavior, but it might be useful to see the type of error that occurred.

> **Good Practice:** You should never use an empty catch statement like this in production code because it "swallows" exceptions and hides potential problems. You should at least log the exception if you cannot or do not want to handle it properly, or rethrow it so that higher-level code can decide instead. You will learn about logging in *Chapter 4, Writing, Debugging, and Testing Functions*.

## Catching all exceptions

To get information about any type of exception that might occur, you can declare a variable of type System.Exception to the catch block:

- 1. Add an exception variable declaration to the catch block and use it to write information about the exception to the console, as shown in the following code:

```
catch (Exception ex)
{
    WriteLine($"{ex.GetType()} says {ex.Message}");
}
```

- 2. Run the code, enter Kermit again, and view the result, as shown in the following output:

```
Before parsing
What is your age? Kermit
System.FormatException says Input string was not in a correct format.
After parsing
```

## Catching specific exceptions

Now that we know which specific type of exception occurred, we can improve our code by catching just that type of exception and customizing the message that we display to the user:

- 1. Leave the existing catch block, and above it, add a new catch block for the format exception type, as shown in the following highlighted code:

```
catch (FormatException)
{
    WriteLine("The age you entered is not a valid number format.");
}
```
