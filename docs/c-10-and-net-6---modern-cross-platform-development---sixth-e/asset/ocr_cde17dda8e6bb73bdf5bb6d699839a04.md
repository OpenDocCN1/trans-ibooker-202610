# Extending types when you can't inherit

Earlier, we saw how the sealed modifier can be used to prevent inheritance.

Microsoft has applied the sealed keyword to the System.String class so that no one can inherit and potentially break the behavior of strings.

Can we still add new methods to strings? Yes, if we use a language feature named extension methods, which was introduced with C# 3.0.

# Using static methods to reuse functionality

Since the first version of C#, we've been able to create static methods to reuse functionality, such as the ability to validate that a string contains an email address. The implementation will use a regular expression that you will learn more about in Chapter 8, Working with Common .NET Types.

Let's write some code:

+   1. In the PacktLibrary project, add a new class named StringExtensions, as shown in the following code, and note the following:
+      - The class imports a namespace for handling regular expressions.
+      - The IsValidEmail method is static and it uses the Regex type to check for matches against a simple email pattern that looks for valid characters before and after the @ symbol.
+   2. In Program.cs, add statements to validate two examples of email addresses, as shown in the following code:

```
using System.Text.RegularExpressions;

namespace Packt.Shared;

public class StringExtensions
{
    public static bool IsValidEmail(string input)
    {
        // use simple regular expression to check
        // that the input string is a valid email
        return Regex.IsMatch(input,
            @"[a-zA-Z0-9\.-_]+@[a-zA-Z0-9\.-_]+"
        );
    }
}
```

```
string email1 = "pamela@test.com";
string email2 = "ian&test.com";
```