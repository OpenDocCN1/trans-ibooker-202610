Comments are written in English (or whatever language the programmer speaks), allowing them to convey information to a degree that variable, function, and class names cannot. But writing concise, effective comments is hard. Comments, like the code, require rewrites and multiple iterations to get right. We understand the code we write immediately after writing it, so writing comments seems like pointless extra work. As a result, programmers are primed to accept the “comments are unnecessary” viewpoint.

The far more common experience is that programs have too few or no comments rather than too many or misleading comments. Rejecting comments is like saying, “Flying across the Atlantic Ocean in a passenger jet is only 99.999991 percent safe, so I’m going to swim across it instead.”

Chapter 10 has more information about how to write effective comments.

# Summary

Code smells indicate that there might be a better way to write your code. They don’t necessarily require a change, but they should make you take another look. The most common code smell is duplicate code, which can signal an opportunity to place code inside a function or loop. This ensures future code changes will need to be made in only one place. Other code smells include magic numbers, which are unexplained values in your code that can be replaced by constants with descriptive names. Similarly, commented-out code and dead code are never run by the computer, and might mislead programmers who later read the program’s code. It’s best to remove them and rely on a source control system like Git if you later need to add them back to your program.

Print debugging uses print() calls to display debugging information. Although this approach to debugging is easy, it’s often faster in the long run to rely on a debugger and logs to diagnose bugs.

Variables with numeric suffixes, such as x1, x2, x3, and so on, are often best replaced with a single variable containing a list. Unlike in languages such as Java, in Python we use modules rather than classes to group functions together. A class that contains a single method or only static methods is a code smell suggesting that you should put that code into a module rather than a class. And although list comprehensions are a concise way to create list values, nested list comprehensions are usually unreadable.

Additionally, any exceptions handled with empty except blocks are a code smell that you’re simply silencing the error rather than handling it. A short, cryptic error message is just as useless to the user as no error message.

Along with these code smells are the code smell myths: programming advice that is no longer valid or has, over time, proven counterproductive. These include putting only a single return statement or try-except block in each function, never using flag arguments or global variables, and believing that comments are unnecessary.