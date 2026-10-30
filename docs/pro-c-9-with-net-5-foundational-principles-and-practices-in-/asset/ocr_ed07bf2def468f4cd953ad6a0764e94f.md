```
case 3:
    //yet another action
    goto default;
default:
    //default action
    break;
}
}
```

# Performing switch Statement Pattern Matching (New 7.0, Updated 9.0)

Prior to C# 7, match expressions in switch statements were limited to comparing a variable to constant values, sometimes referred to as the constant pattern. In C# 7, switch statements can also employ the type pattern, where case statements can evaluate the type of the variable being checked and case expressions are no longer limited to constant values. The rule that each case statement must be terminated with a return or break still applies; however, goto statements are not supported using the type pattern.

> Note If you are new to object-oriented programming, this section might be a little confusing. It will all come together in Chapter 6, when you revisit the new pattern matching features of C# 7 in the context of classes and base classes. For now, just understand that there is a powerful new way to write switch statements.

Add another method named ExecutePatternMatchingSwitch() and add the following code:

```
static void ExecutePatternMatchingSwitch()
{
    Console.WriteLine("1 [Integer (5)], 2 [String (\"Hi\")], 3 [Decimal (2.5)]");
    Console.Write("Please choose an option: ");
    string userChoice = Console.ReadLine();
    object choice;
    //This is a standard constant pattern switch statement to set up the example
    switch (userChoice)
    {
        case "1":
            choice = 5;
            break;
        case "2":
            choice = "Hi";
            break;
        case "3":
            choice = 2.5;
            break;
        default:
            choice = 5;
            break;
    }
    //This is new the pattern matching switch statement
    switch (choice)
```