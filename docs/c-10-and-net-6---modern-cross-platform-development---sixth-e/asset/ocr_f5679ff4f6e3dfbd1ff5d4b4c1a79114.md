1. Add statements to declare a dynamic variable and then assign a string literal value, and then an integer value, and then an array of integer values, as shown in the following code:

```
// storing a string in a dynamic object
// string has a Length property
dynamic something = "Ahmed";

// int does not have a Length property
// something = 12;

// an array of any type has a Length property
// something = new[] { 3, 5, 7 };
```

2. Add a statement to output the length of the dynamic variable, as shown in the following code:

```
// this compiles but would throw an exception at run-time
// if you later store a data type that does not have a
// property named Length
Console.WriteLine($"Length is {something.Length}");
```

3. Run the code and note it works because a string value does have a Length property, as shown in the following output:

```
Length is 5
```

4. Uncomment the statement that assigns an int value.

5. Run the code and note the runtime error because int does not have a Length property, as shown in the following output:

```
Unhandled exception. Microsoft.CSharp.RuntimeBinder.
RuntimeBinderException: 'int' does not contain a definition for 'Length'
```

6. Uncomment the statement that assigns the array.

7. Run the code and note the output because an array of three int values does have a Length property, as shown in the following output:

```
Length is 3
```

One limitation of dynamic is that code editors cannot show IntelliSense to help you write the code. This is because the compiler cannot check what the type is during build time. Instead, the CLR checks for the member at runtime and throws an exception if it is missing.

Exceptions are a way to indicate that something has gone wrong at runtime. You will learn more about them and how to handle them in Chapter 3, Controlling Flow, Converting Types, and Handling Exceptions.