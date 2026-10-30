Alternatively, you could now write code like the following:

```
class SimpleMath
{
    public int Add(int x, int y) => x + y;
    public void PrintSum(int x, int y) => Console.WriteLine(x + y);
}
```

Ideally, at this point, you can see the overall role of lambda expressions and understand how they provide a “functional manner” to work with anonymous methods and delegate types. Although the lambda operator (=>) might take a bit to get used to, always remember a lambda expression can be broken down to the following simple equation:

```
ArgumentsToProcess =>
{
    //StatementsToProcessThem
}
```

Or, if using the => operator to implement a single-line type member, it would be like this:

```
TypeMember => SingleCodeStatement
```

It is worth pointing out that the LINQ programming model also makes substantial use of lambda expressions to help simplify your coding efforts. You will examine LINQ beginning in Chapter 13.

# Summary

In this chapter, you examined several ways in which multiple objects can partake in a bidirectional conversation. First, you looked at the C# delegate keyword, which is used to indirectly construct a class derived from System.MulticastDelegate. As you saw, a delegate object maintains the method to call when told to do so.

You then examined the C# event keyword, which, when used in conjunction with a delegate type, can simplify the process of sending your event notifications to waiting callers. As shown via the resulting CIL, the .NET event model maps to hidden calls on the System.Delegate/System.MulticastDelegate types. In this light, the C# event keyword is purely optional, in that it simply saves you some typing time. As well, you have seen that the C# 6.0 null conditional operator simplifies how you safely fire events to any interested party.

This chapter also explored a C# language feature termed anonymous methods. Using this syntactic construct, you can directly associate a block of code statements to a given event. As you have seen, anonymous methods are free to ignore the parameters sent by the event and have access to the “outer variables” of the defining method. You also examined a simplified way to register events using method group conversion.

Finally, you wrapped things up by looking at the C# lambda operator, =>. As shown, this syntax is a great shorthand notation for authoring anonymous methods, where a stack of arguments can be passed into a group of statements for processing. Any method in the .NET Core platform that takes a delegate object as an argument can be substituted with a related lambda expression, which will typically simplify your code base quite a bit.