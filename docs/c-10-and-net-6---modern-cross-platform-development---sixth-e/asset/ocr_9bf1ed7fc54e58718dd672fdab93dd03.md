```
WriteLine($"a =      {ToBinaryString(a)}");
WriteLine($"b =      {ToBinaryString(b)}");
WriteLine($"a & b = {ToBinaryString(a & b)}");
WriteLine($"a | b = {ToBinaryString(a | b)}");
WriteLine($"a ^ b = {ToBinaryString(a ^ b)}");
```

3. Run the code and note the results, as shown in the following output:

```
Outputting integers as binary:
a =      00001010
b =      00000110
a & b = 00000010
a | b = 00001110
a ^ b = 00001100
```

# Miscellaneous operators

nameof and sizeof are convenient operators when working with types:

+   - nameof returns the short name (without the namespace) of a variable, type, or member as a string value, which is useful when outputting exception messages.
+   - sizeof returns the size in bytes of simple types, which is useful for determining the efficiency of data storage.

There are many other operators; for example, the dot between a variable and its members is called the member access operator and the round brackets at the end of a function or method name are called the invocation operator, as shown in the following code:

```
int age = 47;

// How many operators in the following statement?
char firstDigit = age.ToString()[0];

// There are four operators:
// = is the assignment operator
// . is the member access operator
// () is the invocation operator
// [] is the indexer access operator
```

# Understanding selection statements

Every application needs to be able to select from choices and branch along different code paths. The two selection statements in C# are if and switch. You can use if for all your code, but switch can simplify your code in some common scenarios such as when there is a single variable that can have multiple values that each require different processing.