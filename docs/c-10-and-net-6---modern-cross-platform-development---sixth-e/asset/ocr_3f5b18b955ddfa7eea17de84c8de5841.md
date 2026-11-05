In the preceding code, note the following:

+   - TimesTable must have a byte value passed to it as a parameter named number.
+   - TimesTable is a static method because it will be called by the static method Main.
+   - TimesTable does not return a value to the caller, so it is declared with the void keyword before its name.
+   - TimesTable uses a for statement to output the times table for the number passed to it.

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