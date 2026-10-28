Python uses the “^” symbol for “exclusive-or.”

```
boolean_1 ^ boolean_2
```

returns True if exactly one of boolean_1 and boolean_2 is True, and False otherwise. (On my US English keyboard, “^” is Shift-6.)

# Exercise 2.4

Complete the truth table in Figure 2.3 for exclusive-or “^”.

|  | boolean_2 |  |
|---|---|---|
| ^ | False | True |
| boolean_1 False |  |  |
| boolean_1 True |  |  |

Figure 2.3: Incomplete Boolean truth table for “xor”

Numeric comparisons are a common source of Boolean results used in conditional statements.

```
3 < 4
```

True

```
-3 <= -4
```

False

```
20 > -6
```

True

```
0 >= 0.01
```

False

These examples show “less than,” “less than or equal,” “greater than,” and “greater than or equal.”