
Your functions should be small and simple, but this doesn’t mean they should always be limited to doing “one thing” (however you define that). It’s fine if your functions have more than one try-except statement and the statements don’t envelop all of the function’s code.

## Myth: Flag Arguments Are Bad

Boolean arguments to function or method calls are sometimes referred to as flag arguments. In programming, a flag is a value that indicates a binary setting, such as “enabled” or “disabled,” and it’s often represented by a Boolean value. We can describe these settings as set (that is, True) or cleared (that is, False).

The false belief that flag arguments to function calls are bad is based on the claim that, depending on the flag value, the function does two entirely different things, such as in the following example:

```
def someFunction(flagArgument):
    if flagArgument:
        # Run some code...
    else:
        # Run some completely different code...
```

Indeed, if your function looks like this, you should create two separate functions rather than making an argument decide which half of the function’s code to run. But most functions with flag arguments don’t do this. For example, you can pass a Boolean value for the sorted() function’s reverse keyword argument to determine the sort order. This code wouldn’t be improved by splitting the function into two functions named sorted() and reverseSorted() (while also duplicating the amount of documentation required). So the idea that flag arguments are always bad is a code smell myth.

## Myth: Global Variables Are Bad

Functions and methods are like mini-programs within your program: they contain code, including local variables that are forgotten when the function returns. This is similar to how a program’s variables are forgotten after it terminates. Functions are isolated: their code either performs correctly or has a bug depending on the arguments passed when they’re called.

But functions and methods that use global variables lose some of this helpful isolation. Every global variable you use in a function effectively becomes another input to the function, just like the arguments. More arguments mean more complexity, which in turn means a higher likelihood for bugs. If a bug manifests in a function due to a bad value in a global variable, that bad value could have been set anywhere in the program. To search for a likely cause of this bad value, you can’t just analyze the code in the function or the line of code calling the function; you must look at the entire program’s code. For this reason, you should limit your use of global variables.

For example, let’s look at the calculateSlicesPerGuest() function in a fictional partyPlanner.py program that is thousands of lines long. I've included line numbers to give you a sense of the program's size:

```
1504. def calculateSlicesPerGuest(numberOfCakeSlices):
1505.     global numberOfPartyGuests
1506.     return numberOfCakeSlices / numberOfPartyGuests
```

Let’s say when we run this program, we encounter the following exception:

```
Traceback (most recent call last):
    File "partyPlanner.py", line 1898, in <module>
        print(calculateSlicesPerGuest(42))
    File "partyPlanner.py", line 1506, in calculateSlicesPerGuest
        return numberOfCakeSlices / numberOfPartyGuests
ZeroDivisionError: division by zero
```

The program has a zero divide error, caused by the line return numberOfCakeSlices / numberOfPartyGuests. The numberOfPartyGuests variable must be set to 0 to have caused this, but where did numberOfPartyGuests get assigned this value? Because it's a global variable, it could have happened anywhere in the thousands of lines in this program! From the traceback information, we know that calculateSlicesPerGuest() was called on line 1898 of our fictional program. If we looked at line 1898, we could find out what argument was passed for the numberOfCakeSlices parameter. But the numberOfPartyGuests global variable could have been set at any time before the function call.

Note that global constants aren't considered poor programming practice. Because their values never change, they don't introduce complexity into the code the way other global variables do. When programmers mention that “global variables are bad,” they aren't referring to constant variables.

Global variables broaden the amount of debugging needed to find where an exception-causing value could have been set. This makes abundant use of global variables a bad idea. But the idea that all global variables are bad is a code smell myth. Global variables can be useful in smaller programs or for keeping track of settings that apply across the entire program. If you can avoid using a global variable, that's a sign you probably should avoid using one. But “global variables are bad” is an oversimplified opinion.

## Myth: Comments Are Unnecessary

Bad comments are indeed worse than no comments at all. A comment with outdated or misleading information creates more work for the programmer instead of a better understanding. But this potential problem is sometimes used to proclaim that all comments are bad. The argument asserts that every comment should be replaced with more readable code, to the point that programs should have no comments at all.

Comments are written in English (or whatever language the programmer speaks), allowing them to convey information to a degree that variable, function, and class names cannot. But writing concise, effective comments is hard. Comments, like the code, require rewrites and multiple iterations to get right. We understand the code we write immediately after writing it, so writing comments seems like pointless extra work. As a result, programmers are primed to accept the “comments are unnecessary” viewpoint.

The far more common experience is that programs have too few or no comments rather than too many or misleading comments. Rejecting comments is like saying, “Flying across the Atlantic Ocean in a passenger jet is only 99.999991 percent safe, so I’m going to swim across it instead.”

Chapter 10 has more information about how to write effective comments.

## Summary

Code smells indicate that there might be a better way to write your code. They don’t necessarily require a change, but they should make you take another look. The most common code smell is duplicate code, which can signal an opportunity to place code inside a function or loop. This ensures future code changes will need to be made in only one place. Other code smells include magic numbers, which are unexplained values in your code that can be replaced by constants with descriptive names. Similarly, commented-out code and dead code are never run by the computer, and might mislead programmers who later read the program’s code. It’s best to remove them and rely on a source control system like Git if you later need to add them back to your program.

Print debugging uses print() calls to display debugging information. Although this approach to debugging is easy, it’s often faster in the long run to rely on a debugger and logs to diagnose bugs.

Variables with numeric suffixes, such as x1, x2, x3, and so on, are often best replaced with a single variable containing a list. Unlike in languages such as Java, in Python we use modules rather than classes to group functions together. A class that contains a single method or only static methods is a code smell suggesting that you should put that code into a module rather than a class. And although list comprehensions are a concise way to create list values, nested list comprehensions are usually unreadable.

Additionally, any exceptions handled with empty except blocks are a code smell that you’re simply silencing the error rather than handling it. A short, cryptic error message is just as useless to the user as no error message.

Along with these code smells are the code smell myths: programming advice that is no longer valid or has, over time, proven counterproductive. These include putting only a single return statement or try-except block in each function, never using flag arguments or global variables, and believing that comments are unnecessary.
