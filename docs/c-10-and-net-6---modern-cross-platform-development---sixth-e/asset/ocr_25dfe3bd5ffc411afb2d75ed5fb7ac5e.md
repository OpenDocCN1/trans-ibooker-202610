2. Run the code and view the result, noting that your output for the date and time might be formatted differently if you are not running it in the UK, and that null values output as an empty string, as shown in the following output:

```
default(int) = 0
default(bool) = False
default(DateTime) = 01/01/0001 00:00:00
default(string) =
```

3. Add statements to declare a number, assign a value, and then reset it to its default value, as shown in the following code:

```
int number = 13;
Console.WriteLine($"number has been set to: {number}");
number = default;
Console.WriteLine($"number has been reset to its default: {number}");
```

4. Run the code and view the result, as shown in the following output:

```
number has been set to: 13
number has been reset to its default: 0
```

# Storing multiple values in an array

When you need to store multiple values of the same type, you can declare an array. For example, you may do this when you need to store four names in a string array.

The code that you will write next will allocate memory for an array for storing four string values. It will then store string values at index positions 0 to 3 (arrays usually have a lower bound of zero, so the index of the last item is one less than the length of the array).

![](bbox=[0.15, 0.62, 0.22, 0.68])

> Good Practice: Do not assume that all arrays count from zero. The most common type of array in .NET is an szArray, a single-dimension zero-indexed array, and these use the normal [] syntax. But .NET also has mdArray, a multi-dimensional array, and they do not have to have a lower bound of zero. These are rarely used but you should know they exist.

Finally, it will loop through each item in the array using a for statement, something that we will cover in more detail in Chapter 3, Controlling Flow, Converting Types, and Handling Exceptions.

Let's look at how to use an array:

1. Type statements to declare and use an array of string values, as shown in the following code:

```
string[] names; // can reference any size array of strings

// allocating memory for four strings in an array
```