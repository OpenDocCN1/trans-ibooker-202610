
Think of the semicolon as equivalent to a newline, and it all becomes much clearer (and more obviously not what you wanted):

```
cd /tmp || echo "cd to /tmp failed."
exit
```

How can we get the behavior we want? We can group the echo and exit together so that they are, taken together, the clause on the righthand side of the “or,” like this:

```
# Succeed with the cd or bail with a message
cd /tmp || { echo "cd to /tmp failed." ; exit ; }
```

The braces are bash syntax for a compound command, i.e., grouping statements together. You may have seen something similar using parentheses, but using parentheses executes the statements in a subshell, also referred to as a child process. That would incur an overhead we don’t need, and the exit occurring from within a subshell wouldn’t accomplish much either.

![](img/553ffa1bc15093729b691f706befc0ac_19_0.png)

## Closing Compound Commands

A quirk of bash requires a specific syntax for how you close the compound command. It must end with either a semicolon or a newline before the closing brace. If you use a semicolon, there needs to be whitespace before the brace so that the brace can be recognized as a reserved word (otherwise it gets confused with the closing brace of shell variable syntax, as in ${VAR}, for example). That is why the preceding example ends with what looks to be an extraneous semicolon: { echo "..." ; exit ; }. Using a newline, that final semicolon isn’t needed:

```
# Succeed with the cd or bail with a message
cd /tmp || { echo "cd to /tmp failed." ; exit
}
```

but that might not read as cleanly. At the left edge, it seems oddly placed; indented with whitespace, it groups more logically but seems bare.

We recommend you stick with the extra semicolon, and don’t forget the space between it and the closing brace.

## More than One Again

What if you need more complex logic? What about multiple AND and OR constructs? How are they handled? What do you think the following line of code will do?

```
[ -n "$DIR" ] && [ -d "$DIR" ] && cd "$DIR" || exit 4
```

If the DIR variable is not empty and the file named by the DIR variable is a directory, then it will cd to that directory; otherwise it will exit from the script, returning a 4.

That does what you might have expected—but maybe not for the reason you think. You might, from this example, be tempted to think that the && has higher precedence than the || operator, but it doesn’t. They’re just grouping from left to right. The syntax for bash says that the && and || operators are of equal precedence and are left associative. Need convincing? Look at these examples:

```
$ echo 1 && echo 2 || echo 3
1
2
$
```

but also:

```
$ echo 1 || echo 2 && echo 3
1
3
$
```

Notice that it always evaluates the leftmost operator regardless of whether it’s AND or OR; it’s not operator precedence but simple left associativity that determines the order of evaluation.

## Don’t Do This

While we’re on the subject of if statements (or how not to use them), here’s an example of an explicit if that you may see quite often in older scripts. We show it to you here so that we can explain the idiom but also to urge you never to imitate this style. Here’s the code:

```
### Don't write your if statements to check like this
if [ $VAR"X" = X ]; then
    echo empty
fi
```

```
### Or this
if [ "x$VAR" == x ]; then
    echo empty
fi
```

```
### Or other variations on this theme
```

Don’t do this. What are they doing in this code? They’re checking to see if the variable VAR is empty. They do that by appending the value of VAR with some character (here X). If the resulting string matches just the letter itself, then the variable was empty. Don’t do this.

There are better ways to make this check. Here’s a simple alternative:

```
# Is the length of the variable zero? i.e., empty or null
if [[ -z "$VAR" ]]; then
    echo empty
fi
```

![](img/553ffa1bc15093729b691f706befc0ac_21_0.png)

## Single Versus Double Brackets

This example of what not to do uses the single brackets, [ and ], to surround the condition that they are testing. That’s not what we’re asking you to avoid. We want you to avoid the string append and comparison; instead, use the -z or -n to make these tests. So why have our examples all used double brackets, [[ and ]], for our if (and non-if) statements? They are an addition to bash (not in the original sh command), and they avoid some confusing edge case behaviors that single brackets exhibit (variable inside quotes or not). We showed this example with single brackets as this type of comparison is often seen in older scripts. You may need to stay with single brackets, if your goal is portability across various platforms and/or to non-bash platforms (e.g., dash). As a side note, the double brackets are keywords, whereas the left single bracket is a builtin, a difference that may explain some subtle differences in behaviors. Our advice remains to use the double-bracket syntax except when unavoidable.

You can check for the opposite case, checking to see if the length of the string is nonzero, by using the -n option or by just referencing the variable:

```
# This checks for a nonzero length, i.e., not empty, not null
if [[ -n "$VAR" ]]; then
    echo "VAR has a value:" $VAR
fi
```

```
# Same here
if [[ "$VAR" ]]; then
    echo even easier this way
fi
```

So you see there is no need to use that other approach, which was necessary in legacy versions of the test command ("[") that are rarely used anymore. We thought you ought to see it, though, so you’ll recognize it in older scripts. Now you also know a better way to write it.

## Style and Readability: Recap

In this chapter we took a look at a particular bash idiom—the “no-if” if statement. It doesn’t look like a traditional if/then/else, but it can behave exactly like one. Unless this is something you recognize, some scripts that you read might remain obscure. This idiom is also worth using to check that any necessary preconditions are in place before executing a command, or to make a short error check without disrupting the flow of the main logic of the script.

By using && and || operators, you can write if/then/else logic without the use of those familiar keywords. But bash does have if, then, and else as keywords. So when do you use them, and when do you use the shorthand? The answer comes down to readability.

For complex logic, it makes the most sense to use the familiar keywords. But, for simple test and check situations with single actions, using the && and || operators can be very convenient and will not distract from the main flow of logic. Use help test to remind you which tests you can use, like -n -r, and consider copying the help text into a comment for the future.

In either case, for familiar if statements or idiomatic “no-if” statements, we encourage the use of the double-bracket syntax.

Now that you’ve seen one bash idiom in depth, let’s take a look at others and really up your bash game.

## Looping Lingo

It’s not just C-style for loops—bash includes other syntaxes and styles; some are more familiar to Python programmers, but each has its place. There is a for loop with no apparent arguments, useful in both scripts and inside functions. There’s also an iterator-like for loop with explicit values and values that can come from other commands.

## Looping Constructs

Looping constructs are common in programming languages. Since the invention of the C language, many programming languages have adopted the C-style for loop. It’s such a powerful, readable construct because it groups the initialization code, the termination condition, and the iteration code all into one place. For example, in C (or Java, or...):

```
/* NOT bash */
for (i=0; i<10; i++) {
    printf("%d\n", i);
}
```

With just a few minor syntax differences, bash follows much the same approach:

```
for ((i=0; i<10; i++)); do
    printf '%d\n' "$i"
done
```

Note, especially, the use of double parentheses. Rather than braces, bash uses do and done to enclose the statements of the loop. As with C/C++, an idiomatic use of the for loop is the empty for loop, giving a deliberate infinite loop (you’ll also see while true; do):
