
## Using target-typed new to instantiate objects

With C# 9, Microsoft introduced another syntax for instantiating objects known as **target-typed new**. When instantiating an object, you can specify the type first and then use new without repeating the type, as shown in the following code:

```
XmlDocument xml3 = new(); // target-typed new in C# 9 or later
```

If you have a type with a field or property that needs to be set, then the type can be inferred, as shown in the following code:

```
class Person
{
    public DateTime BirthDate;
}

Person kim = new();
kim.BirthDate = new(1967, 12, 26); // instead of: new DateTime(1967, 12, 26)
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_108_0.png)

> **Good Practice:** Use target-typed new to instantiate objects unless you must use a pre-version 9 C# compiler. I have used target-typed new throughout the rest of this book. Please let me know if you spot any cases that I missed!

## Getting and setting the default values for types

Most of the primitive types except string are **value types**, which means that they must have a value. You can determine the default value of a type by using the default() operator and passing the type as a parameter. You can assign the default value of a type by using the default keyword.

The string type is a **reference type**. This means that string variables contain the memory address of a value, not the value itself. A reference type variable can have a null value, which is a literal that indicates that the variable does not reference anything (yet). null is the default for all reference types.

You'll learn more about value types and reference types in *Chapter 6, Implementing Interfaces and Inheriting Classes*.

Let's explore default values:

- 1. Add statements to show the default values of an int, bool, DateTime, and string, as shown in the following code:

```
Console.WriteLine($"default(int) = {default(int)}");
Console.WriteLine($"default(bool) = {default(bool)}");
Console.WriteLine($"default(DateTime) = {default(DateTime)}");
Console.WriteLine($"default(string) = {default(string)}");
```

- 2. Run the code and view the result, noting that your output for the date and time might be formatted differently if you are not running it in the UK, and that null values output as an empty string, as shown in the following output:

```
default(int) = 0
default(bool) = False
default(DateTime) = 01/01/0001 00:00:00
default(string) =
```

- 3. Add statements to declare a number, assign a value, and then reset it to its default value, as shown in the following code:

```
int number = 13;
Console.WriteLine($"number has been set to: {number}");
number = default;
Console.WriteLine($"number has been reset to its default: {number}");
```

- 4. Run the code and view the result, as shown in the following output:

```
number has been set to: 13
number has been reset to its default: 0
```

## Storing multiple values in an array

When you need to store multiple values of the same type, you can declare an array. For example, you may do this when you need to store four names in a string array.

The code that you will write next will allocate memory for an array for storing four string values. It will then store string values at index positions 0 to 3 (arrays usually have a lower bound of zero, so the index of the last item is one less than the length of the array).

![](img/c63b00dca7192cc58cfb9e1f1e64f483_109_0.png)

> Good Practice: Do not assume that all arrays count from zero. The most common type of array in .NET is an szArray, a single-dimension zero-indexed array, and these use the normal [] syntax. But .NET also has mdArray, a multi-dimensional array, and they do not have to have a lower bound of zero. These are rarely used but you should know they exist.

Finally, it will loop through each item in the array using a for statement, something that we will cover in more detail in Chapter 3, Controlling Flow, Converting Types, and Handling Exceptions.

Let's look at how to use an array:

- 1. Type statements to declare and use an array of string values, as shown in the following code:

```
string[] names; // can reference any size array of strings

// allocating memory for four strings in an array
names = new string[4];

// storing items at index positions
names[0] = "Kate";
names[1] = "Jack";
names[2] = "Rebecca";
names[3] = "Tom";

// looping through the names
for (int i = 0; i < names.Length; i++)
{
    // output the item at index position i
    Console.WriteLine(names[i]);
}
```

- 2. Run the code and note the result, as shown in the following output:

```
Kate
Jack
Rebecca
Tom
```

Arrays are always of a fixed size at the time of memory allocation, so you need to decide how many items you want to store before instantiating them.

An alternative to defining the array in three steps as above is to use array initializer syntax, as shown in the following code:

```
string[] names2 = new[] { "Kate", "Jack", "Rebecca", "Tom" };
```

When you use the new[] syntax to allocate memory for the array, you must have at least one item in the curly braces so that the compiler can infer the data type.

Arrays are useful for temporarily storing multiple items, but collections are a more flexible option when adding and removing items dynamically. You don't need to worry about collections right now, as we will cover them in Chapter 8, Working with Common .NET Types.

## Exploring more about console applications

We have already created and used basic console applications, but we're now at a stage where we should delve into them more deeply.

Console applications are text-based and are run at the command line. They typically perform simple tasks that need to be scripted, such as compiling a file or encrypting a section of a configuration file.

Equally, they can also have arguments passed to them to control their behavior.

An example of this would be to create a new console app using the F# language with a specified name instead of using the name of the current folder, as shown in the following command line:

```bash
dotnet new console -lang "F#" --name "ExploringConsole"
```

## Displaying output to the user

The two most common tasks that a console application performs are writing and reading data. We have already been using the WriteLine method to output, but if we didn't want a carriage return at the end of the lines, we could have used the Write method.

## Formatting using numbered positional arguments

One way of generating formatted strings is to use numbered positional arguments.

This feature is supported by methods like Write and WriteLine, and for methods that do not support the feature, the string parameter can be formatted using the Format method of string.

> The first few code examples in this section will work with a .NET Interactive notebook because they are about outputting to the console. Later in this section, you will learn about getting input via the console and sadly notebooks do not support this.

Let's begin formatting:

- 1. Use your preferred code editor to add a new Console Application to the Chapter02 workspace/solution named Formatting.
- 2. In Visual Studio Code, select Formatting as the active OmniSharp project.
- 3. In Program.cs, type statements to declare some number variables and write them to the console, as shown in the following code:

```csharp
int numberOfApples = 12;
decimal pricePerApple = 0.35M;

Console.WriteLine(
    format: "{0} apples costs {1:C}",
    arg0: numberOfApples,
    arg1: pricePerApple * numberOfApples);

string formatted = string.Format(
    format: "{0} apples costs {1:C}",
    arg0: numberOfApples,
    arg1: pricePerApple * numberOfApples);

//WriteToFile(formatted); // writes the string into a file
```

The WriteToFile method is a nonexistent method used to illustrate the idea.

![](img/c63b00dca7192cc58cfb9e1f1e64f483_112_0.png)

> **Good Practice:** Once you become more comfortable with formatting strings, you should stop naming the parameters, for example, stop using format:, arg0:, and arg1:. The preceding code uses a non-canonical style to show where the 0 and 1 came from while you are learning.

## Formatting using interpolated strings

C# 6.0 and later have a handy feature named interpolated strings. A string prefixed with $ can use curly braces around the name of a variable or expression to output the current value of that variable or expression at that position in the string, as the following shows:

- 1. Enter a statement at the bottom of the Program.cs file, as shown in the following code:
- 2. Run the code and view the result, as shown in the following partial output:

```
Console.WriteLine($"{numberOfApples} apples costs {pricePerApple * numberOfApples:C}");
```

```
12 apples costs £4.20
```

For short, formatted string values, an interpolated string can be easier for people to read. But for code examples in a book, where lines need to wrap over multiple lines, this can be tricky. For many of the code examples in this book, I will use numbered positional arguments.

Another reason to avoid interpolated strings is that they can't be read from resource files to be localized.

Before C# 10, string constants could only be combined by using concatenation, as shown in the following code:

```
private const string firstname = "Omar";
private const string lastname = "Rudberg";
private const string fullname = firstname + " " + lastname;
```

With C# 10, interpolated strings can now be used, as shown in the following code:

```
private const string fullname = "{firstname} {lastname}";
```

This only works for combining string constant values. It cannot work with other types like numbers that would require runtime data type conversions.

## Understanding format strings

A variable or expression can be formatted using a format string after a comma or colon.

An N0 format string means a number with a thousand separators and no decimal places, while a C format string means currency. The currency format will be determined by the current thread.
