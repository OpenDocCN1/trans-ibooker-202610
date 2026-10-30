```
{
    case int i:
        Console.WriteLine("Your choice is an integer.");
        break;
    case string s:
        Console.WriteLine("Your choice is a string.");
        break;
    case decimal d:
        Console.WriteLine("Your choice is a decimal.");
        break;
    default:
        Console.WriteLine("Your choice is something else");
        break;
}
Console.WriteLine();
}
```

The first switch statement is using the standard constant pattern and is included merely to set up this (trivial) example. In the second switch statement, the variable is typed as object and, based on the input from the user, can be parsed into an int, string, or decimal data type. Based on the type of the variable, different case statements are matched. In addition to checking the data type, a variable is assigned in each of the case statements (except for the default case). Update the code to the following to use the values in the variables:

```
//This is new the pattern matching switch statement
switch (choice)
{
    case int i:
        Console.WriteLine("Your choice is an integer {0}.",i);
        break;
    case string s:
        Console.WriteLine("Your choice is a string. {0}", s);
        break;
    case decimal d:
        Console.WriteLine("Your choice is a decimal. {0}", d);
        break;
    default:
        Console.WriteLine("Your choice is something else");
        break;
}
```

In addition to evaluating on the type of the match expression, when clauses can be added to the case statements to evaluate conditions on the variable. In this example, in addition to checking the type, the value of the converted type is also checked for a match:

```
static void ExecutePatternMatchingSwitchWithWhen()
{
    Console.WriteLine("1 [C#], 2 [VB]");
    Console.Write("Please pick your language preference: ");

    object langChoice = Console.ReadLine();
```