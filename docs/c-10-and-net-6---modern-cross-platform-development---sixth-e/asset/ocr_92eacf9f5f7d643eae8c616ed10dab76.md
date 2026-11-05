2. Run the code and view the result, as shown in the following output:

```
Using doubles:
0.1 + 0.2 does NOT equal 0.3
```

In locales that use a comma for the decimal separator the result will look slightly different, as shown in the following output:

```
0,1 + 0,2 does NOT equal 0,3
```

The double type is not guaranteed to be accurate because some numbers like 0.1 literally cannot be represented as floating-point values.

As a rule of thumb, you should only use double when accuracy, especially when comparing the equality of two numbers, is not important. An example of this may be when you're measuring a person's height and you will only compare values using greater than or less than, but never equals.

The problem with the preceding code is illustrated by how the computer stores the number 0.1, or multiples of it. To represent 0.1 in binary, the computer stores 1 in the 1/16 column, 1 in the 1/32 column, 1 in the 1/256 column, 1 in the 1/512 column, and so on.

The number 0.1 in decimal is 0.00011001100110011... in binary, repeating forever:

| 4 | 2 | 1 | . | 1/2 | 1/4 | 1/8 | 1/16 | 1/32 | 1/64 | 1/128 | 1/256 | 1/512 | 1/1024 | 1/2048 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---| 
| 0 | 0 | 0 | . | 0 | 0 | 0 | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 |

![](bbox=[0.11, 0.8, 0.18, 0.85])

**Good Practice**: Never compare double values using ==. During the First Gulf War, an American Patriot missile battery used double values in its calculations. The inaccuracy caused it to fail to track and intercept an incoming Iraqi Scud missile, and 28 soldiers were killed; you can read about this at https://www.ima.umn.edu/~arnold/disasters/patriot.html.

+   1. Copy and paste the statements that you wrote before (that used the double variables).
+   2. Modify the statements to use decimal and rename the variables to c and d, as shown in the following code:

```
Console.WriteLine("Using decimals:");
decimal c = 0.1M; // M suffix means a decimal literal value
decimal d = 0.2M;

if (c + d == 0.3M)
{
    Console.WriteLine($"{c} + {d} equals {0.3M}");
}
else
{
```