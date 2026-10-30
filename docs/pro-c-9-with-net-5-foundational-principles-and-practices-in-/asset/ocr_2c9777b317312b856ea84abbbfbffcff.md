```
double.NegativeInfinity);
Console.WriteLine();
}
```

When you define a literal whole number (such as 500), the runtime will default the data type to an int. Likewise, literal floating-point data (such as 55.333) will default to a double. To set the underlying data type to a long, use suffix l or L (4L). To declare a float variable, use the suffix f or F to the raw numerical value (5.3F), and use the suffix m or M to a floating-point number to declare a decimal (300.5M). This becomes more important when declaring variables implicitly, which is covered later in this chapter.

# Understanding the Members of System.Boolean

Next, consider the System.Boolean data type. The only valid assignment a C# bool can take is from the set {true || false}. Given this point, it should be clear that System.Boolean does not support a MinValue/MaxValue property set but rather TrueString/FalseString (which yields the string "True" or "False", respectively). Here is an example:

```
Console.WriteLine("bool.FalseString: {0}", bool.FalseString);
Console.WriteLine("bool.TrueString: {0}", bool.TrueString);
```

# Understanding the Members of System.Char

C# textual data is represented by the string and char keywords, which are simple shorthand notations for System.String and System.Char, both of which are Unicode under the hood. As you might already know, a string represents a contiguous set of characters (e.g., "Hello"), while the char can represent a single slot in a string (e.g., 'H').

The System.Char type provides you with a great deal of functionality beyond the ability to hold a single point of character data. Using the static methods of System.Char, you are able to determine whether a given character is numerical, alphabetical, a point of punctuation, or whatnot. Consider the following method:

```
static void CharFunctionality()
{
    Console.WriteLine("=> char type Functionality:");
    char myChar = 'a';
    Console.WriteLine("char.IsDigit('a'): {0}", char.IsDigit(myChar));
    Console.WriteLine("char.IsLetter('a'): {0}", char.IsLetter(myChar));
    Console.WriteLine("char.IsWhiteSpace('Hello There', 5): {0}",
        char.IsWhiteSpace("Hello There", 5));
    Console.WriteLine("char.IsWhiteSpace('Hello There', 6): {0}",
        char.IsWhiteSpace("Hello There", 6));
    Console.WriteLine("char.IsPunctuation('?'): {0}",
        char.IsPunctuation('?'));
    Console.WriteLine();
}
```

As illustrated in the previous method, many members of System.Char have two calling conventions: a single character or a string with a numerical index that specifies the position of the character to test.