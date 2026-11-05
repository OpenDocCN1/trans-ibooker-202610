```
WriteLine("{0} is a valid e-mail address: {1}",
    arg0: email1,
    arg1: StringExtensions.IsValidEmail(email1));

WriteLine("{0} is a valid e-mail address: {1}",
    arg0: email2,
    arg1: StringExtensions.IsValidEmail(email2));
```

+   3. Run the code and view the result, as shown in the following output:

```
pamela@test.com is a valid e-mail address: True
ian@test.com is a valid e-mail address: False
```

This works, but extension methods can reduce the amount of code we must type and simplify the usage of this function.

# Using extension methods to reuse functionality

It is easy to make static methods into extension methods:

+   1. In StringExtensions.cs, add the static modifier before the class, and add the this modifier before the string type, as highlighted in the following code:

```
public static class StringExtensions
{
    public static bool IsValidEmail(this string input)
    {
```

These two changes tell the compiler that it should treat the method as one that extends the string type.

+   2. In Program.cs, add statements to use the extension method for string values that need to be checked for valid email addresses, as shown in the following code:

```
WriteLine("{0} is a valid e-mail address: {1}",
    arg0: email1,
    arg1: email1.IsValidEmail());

WriteLine("{0} is a valid e-mail address: {1}",
    arg0: email2,
    arg1: email2.IsValidEmail());
```

Note the subtle simplification in the syntax for calling the IsValidEmail method. The older, longer syntax still works too.

+   3. The IsValidEmail extension method now appears to be a method just like all the actual instance methods of the string type, such as IsNormalized and Insert, as shown in Figure 6.6: