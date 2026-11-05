```
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

# Documenting functions with XML comments

By default, when calling a function such as CardinalToOrdinal, code editors will show a tooltip with basic information, as shown in Figure 4.1:

![](bbox=[0.07, 0.56, 0.89, 0.73])

Figure 4.1: A tooltip showing the default simple method signature

Let's improve the tooltip by adding extra information:

+   1. If you are using Visual Studio Code with the C# extension, you should navigate to View | Command Palette | Preferences: Open Settings (UI), and then search for formatOnType and make sure that is enabled. C# XML documentation comments are a built-in feature of Visual Studio 2022.
+   2. On the line above the CardinalToOrdinal function, type three forward slashes ///, and note that they are expanded into an XML comment that recognizes that the function has a single parameter named number.