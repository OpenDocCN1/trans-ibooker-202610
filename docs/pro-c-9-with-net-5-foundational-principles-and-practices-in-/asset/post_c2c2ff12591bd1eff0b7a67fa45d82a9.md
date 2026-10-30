
Rather, type inference keeps the strongly typed aspect of the C# language and affects only the declaration of variables at compile time. After that, the data point is treated as if it were declared with that type; assigning a value of a different type into that variable will result in a compile-time error.

```
static void ImplicitTypingIsStrongTyping()
{
    // The compiler knows "s" is a System.String.
    var s = "This variable can only hold string data!";
    s = "This is fine...";

    // Can invoke any member of the underlying type.
    string upper = s.ToUpper();

    // Error! Can't assign numerical data to a string!
    s = 44;
}
```

## Understanding the Usefulness of Implicitly Typed Local Variables

Now that you have seen the syntax used to declare implicitly typed local variables, I am sure you are wondering when to make use of this construct. First, using var to declare local variables simply for the sake of doing so brings little to the table. Doing so can be confusing to others reading your code because it becomes harder to quickly determine the underlying data type and, therefore, more difficult to understand the overall functionality of the variable. So, if you know you need an int, declare an int!

However, as you will see beginning in Chapter 13, the LINQ technology set makes use of query expressions that can yield dynamically created result sets based on the format of the query itself. In these cases, implicit typing is extremely helpful because you do not need to explicitly define the type that a query may return, which in some cases would be literally impossible to do. Without getting hung up on the following LINQ example code, see whether you can figure out the underlying data type of subset:

```
static void LinqQueryOverInts()
{
    int[] numbers = { 10, 20, 30, 40, 1, 2, 3, 8 };

    // LINQ query!
    var subset = from i in numbers where i < 10 select i;

    Console.Write("Values in subset: ");
    foreach (var i in subset)
    {
        Console.Write("{0} ", i);
    }
    Console.WriteLine();

    // Hmm...what type is subset?
    Console.WriteLine("subset is a: {0}", subset.GetType().Name);
    Console.WriteLine("subset is defined in: {0}", subset.GetType().Namespace);
}
```

You might be assuming that the subset data type is an array of integers. That seems to be the case, but, in fact, it is a low-level LINQ data type that you would never know about unless you have been doing LINQ

for a long time or you open the compiled image in ildasm.exe. The good news is that when you are using LINQ, you seldom (if ever) care about the underlying type of the query’s return value; you will simply assign the value to an implicitly typed local variable.

In fact, it could be argued that the only time you would make use of the var keyword is when defining data returned from a LINQ query. Remember, if you know you need an int, just declare an int! Overuse of implicit typing (via the var keyword) is considered by most developers to be poor style in production code.

## Working with C# Iteration Constructs

All programming languages provide ways to repeat blocks of code until a terminating condition has been met. Regardless of which language you have used in the past, I would guess the C# iteration statements should not raise too many eyebrows and should require little explanation. C# provides the following four iteration constructs:

- for loop
- foreach/in loop
- while loop
- do/while loop

Let’s quickly examine each looping construct in turn, using a new Console Application project named IterationsAndDecisions.

Note I will keep this section of the chapter short and to the point, as I am assuming you have experience using similar keywords (if, for, switch, etc.) in your current programming language. If you require more information, look up the topics “Iteration Statements (C# Reference),” “Jump Statements (C# Reference),” and “Selection Statements (C# Reference)” within the C# documentation.

### Using the for Loop

When you need to iterate over a block of code a fixed number of times, the for statement provides a good deal of flexibility. In essence, you are able to specify how many times a block of code repeats itself, as well as the terminating condition. Without belaboring the point, here is a sample of the syntax:

```
// A basic for loop.
static void ForLoopExample()
{
    // Note! "i" is only visible within the scope of the for loop.
    for(int i = 0; i < 4; i++)
    {
        Console.WriteLine("Number is: {0} ", i);
    }
    // "i" is not visible here.
}
```

All your old C, C++, and Java tricks still hold when building a C# for statement. You can create complex terminating conditions, build endless loops, loop in reverse (via the -- operator), and use the goto, continue, and break jump keywords.

### Using the foreach Loop

The C# foreach keyword allows you to iterate over all items in a container without the need to test for an upper limit. Unlike a for loop, however, the foreach loop will walk the container only in a linear (n+1) fashion (thus, you cannot go backward through the container, skip every third element, or whatnot).

However, when you simply need to walk a collection item by item, the foreach loop is the perfect choice. Here are two examples using foreach—one to traverse an array of strings and the other to traverse an array of integers. Notice that the data type before the in keyword represents the type of data in the container.

```
// Iterate array items using foreach.
static void ForEachLoopExample()
{
    string[] carTypes = {"Ford", "BMW", "Yugo", "Honda"};
    foreach (string c in carTypes)
    {
        Console.WriteLine(c);
    }

    int[] myInts = { 10, 20, 30, 40 };
    foreach (int i in myInts)
    {
        Console.WriteLine(i);
    }
}
```

The item after the in keyword can be a simple array (seen here) or, more specifically, any class implementing the IEnumerable interface. As you will see in Chapter 10, the .NET Core base class libraries ship with a number of collections that contain implementations of common abstract data types (ADTs). Any of these items (such as the generic List<T>) can be used within a foreach loop.

### Using Implicit Typing Within foreach Constructs

It is also possible to use implicit typing within a foreach looping construct. As you would expect, the compiler will correctly infer the correct "type of type." Recall the LINQ example method shown earlier in this chapter. Given that you do not know the exact underlying data type of the subset variable, you can iterate over the result set using implicit typing. Make sure to add the following using statement to the top of the file:

```
using System.Linq;
static void LinqQueryOverInts()
{
    int[] numbers = { 10, 20, 30, 40, 1, 2, 3, 8 };

    // LINQ query!
    var subset = from i in numbers where i < 10 select i;
    Console.Write("Values in subset: ");

    foreach (var i in subset)
    {
        Console.Write("{0} ", i);
    }
}
```

### Using the while and do/while Looping Constructs

The while looping construct is useful should you want to execute a block of statements until some terminating condition has been reached. Within the scope of a while loop, you will need to ensure this terminating event is indeed established; otherwise, you will be stuck in an endless loop. In the following example, the message "In while loop" will be continuously printed until the user terminates the loop by entering yes at the command prompt:

```
static void WhileLoopExample()
{
    string userIsDone = "";

    // Test on a lower-class copy of the string.
    while(userIsDone.ToLower() != "yes")
    {
        Console.WriteLine("In while loop");
        Console.Write("Are you done? [yes] [no]: ");
        userIsDone = Console.ReadLine();
    }
}
```

Closely related to the while loop is the do/while statement. Like a simple while loop, do/while is used when you need to perform some action an undetermined number of times. The difference is that do/while loops are guaranteed to execute the corresponding block of code at least once. In contrast, it is possible that a simple while loop may never execute if the terminating condition is false from the onset.

```
static void DoWhileLoopExample()
{
    string userIsDone = "";

    do
    {
        Console.WriteLine("In do/while loop");
        Console.Write("Are you done? [yes] [no]: ");
        userIsDone = Console.ReadLine();
    }while(userIsDone.ToLower() != "yes"); // Note the semicolon!
}
```

### A Quick Discussion About Scope

Like all languages based on C (C#, Java, etc.), a scope is created using curly braces. You have already seen this in many of the examples so far, including namespaces, classes, and methods. The iteration and decision constructs also operate in a scope, as in the following example:

```
for(int i = 0; i < 4; i++)
{
    Console.WriteLine("Number is: {0} ", i);
}
```
