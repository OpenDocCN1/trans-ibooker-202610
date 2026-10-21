
Email *bookquestions@oreilly.com* to comment or ask technical questions about this book.

For more information about our books, courses, conferences, and news, see our website at *https://www.oreilly.com*.

Find us on Facebook: *https://facebook.com/oreilly*.

Follow us on Twitter: *https://twitter.com/oreillymedia*.

Watch us on YouTube: *https://www.youtube.com/oreillymedia*.

## Acknowledgments

## bash

Thank you to the GNU Software Foundation and Brian Fox for writing bash. And a very big thank-you to Chet Ramey, who has been maintaining and improving bash since around version 1.14 in the early to mid-1990s. You (plural) have given us a great tool that has helped in so many ways.

## Reviewers

Many thanks to our reviewers, Doug McIlroy, Ian Miell, Curtis Old, and Paul Troncone! They all provided valuable feedback, suggestions, and in some cases alternate solutions, pointed out issues we had overlooked, and in general greatly improved the book. Any errors or omissions in this text are ours and not theirs.

## O’Reilly

Thanks to the entire team at O’Reilly, without whom this book would not exist (for many reasons)—and if it did, the content wouldn’t be or look nearly as good!

Thanks to Mike Loukides for the original idea and for asking us and trusting us to run with it. Thanks to Suzanne “Zan” McQuade for helping to flesh it out. Many thanks to Nicole Taché for editing, sanity checks, and generally putting up with us during the long writing process and to Kristen Brown for all the same things during the production process. Special thanks to Nick Adams in Tools for fixing our many (egregious) AsciiDoc bugs, and other above and beyond tool-chain help. Thank you also to Kim Sandoval (copyeditor), Cheryl Lenser (indexer), Liz Wheeler (proofreader), David Futato (interior design), Karen Montgomery (cover design), and the rest of the great team at O’Reilly.

## From Carl

Thanks to JP for all his work on this, his thoroughness, his attention to detail, and his willingness to put up with me. Thanks to all the O’Reilly people for helping to bring this to realization.

My work on this book is dedicated to my wife, Cynthia, who puts up with my strange hours when I’m writing and pretends, convincingly, to be interested in what I’m writing about. My work on this book is meant, as we say at Bethel University, for God’s glory and my neighbor’s good.

## From JP

Thanks to Carl for all his work; we seem to have managed complementary schedules again. Thanks to Mike for getting the ball rolling (again) and to Nicole for keeping it rolling and her patience with our work, life, and time management issues.

This book is dedicated to my wife, Karen, who is the executive vice president in charge of all the things, without whom my life would not function. Thanks for your incredible support, patience, and understanding. Finally, thanks to Kate and Sam, for taking “unless you’re bleeding or on fire, I’ve got book stuff to do” for an answer.

## A Big “if” Idiom

To get you started in understanding bash idioms, we’ll look at a bash construct that allows you to do what you might normally do with an if/then/else construct but with sparser syntax. The idiomatic expression we’ll show you in this chapter has some real advantages—mostly terseness—and some pitfalls to avoid. But if you don’t know bash idioms, you might not recognize or realize what is going on.

Take a look at this piece of code:

```
[[ -n "$DIR" ]] && cd "$DIR"
```

Do you think that looks like an if statement? If you’re conversant in bash, you’ll recognize that it is functionally the same thing; you’ll understand it as an if statement even though the if keyword doesn’t appear in the code.

What’s going on?

## The Big “if”

To explain this idiom, let’s first look at a similar but simpler example:

```
cd tmp && rm scratchfile
```

This too, is, in effect, an if statement. If the cd command succeeds, then (and only then) execute the rm command. The “idiom” here is the use of the double ampersand (&&), typically read as “and,” to separate the two commands.

Logic or philosophy classes teach the rule: the expression “A AND B” is true if and only if both A and B are each true. Therefore if A is false, there is no need to even consider the value of B. For example, consider “I own a dog AND I own a cat.” If I do not own a dog, then this compound expression is false for me, regardless of my cat situation.

Let’s apply this to bash. Remember that the basic function of bash is to execute programs. The first part of this statement has the cd command execute. Similar to the logic of the AND, if this first command fails, bash will not bother to execute the second part, the rm command.

The use of the && is meant to remind you of the AND behavior. Bash isn’t actually performing an “AND” operation on these two results. (If this were C/C++, that would be a different story, though the conditional execution is the same.) However, this bash idiom does provide the conditional execution of the second command—it isn’t run if the first command fails.

Let’s go back and look at the original example we gave, namely this expression:

```
[[ -n "$DIR" ]] && cd "$DIR"
```

Do you think you “get it” now? The first expression tests to see if the length of the value of the variable called DIR is nonzero. If it has a value, that is, if its length is nonzero—and only if it’s nonzero—the cd command will attempt to change into a directory named by the value of DIR.

We could have written this as an explicit if statement:

```
if [[ -n "$DIR" ]]; then
  cd "$DIR"
fi
```

To someone less familiar with bash, this latter format is certainly more readable and easily understood. But there isn’t much going on inside the then clause, just the cd command, so it seems a bit “bulky” in its syntax. You’ll have to decide which to use based on who your readers will likely be and how likely other commands would be added inside the then clause. We’ll offer more opinions on this topic in the sections that follow.

![](img/553ffa1bc15093729b691f706befc0ac_17_0.png)

## bash Help

The bash help command is great for any builtin command, but help test provides especially helpful clues about the test expression, like -n. You might also check man bash, but if you do, you’ll want to search for “conditional expressions.” The bash man page is very long; help thing has much shorter, focused topics. If you are not sure if the command in question is a bash builtin, just try it with the help command, or use type -a thing to find out.

Admittedly, knowing that help test will tell you what -n means is tricky, but then, you were smart enough to buy this book, so now you know. Here is another subtle little tidbit; go try this: help [.
You’re welcome.

## Or ELSE...

A similar idiom is available using the || characters to separate two items in a bash list. Pronounced “or,” the second part will execute only if the first part fails. This is meant to remind you of the logic rule for “OR,” as in: A OR B. The whole expression is true if either A is true or B is true. Put another way, if A is true, it doesn’t matter if B is true or false. For example, consider the phrase “I own a dog OR I own a cat.” If I do, in fact, own a dog, then this expression is true for me regardless of my cat situation.

Applying this to bash:

```
[[ -z "$DIR" ]] || cd "$DIR"
```

Do you think you can explain this one? If the variable is of length zero, then the first part is “true,” so there is no need to execute the second half; no cd command will be run. But if the length of $DIR is nonzero, the test would return “false,” so only then would we run the cd command.

You might read the line of bash as “Either $DIR is zero length, OR we attempt to cd into that directory.”

To write that as an explicit if statement is a bit odd, as there is no then action to be taken. The code after the || is like the else clause:

```
if [[ -z "$DIR" ]]; then
  :
else
  cd "$DIR"
fi
```

The ":" is a null statement in shell—so it does nothing in that case.

In summary: two commands separated by an && are like an if and its then clause; two commands separated by a || are like an if and its else clause.

## More than One

You might want to do more than one thing in this else-like clause after the || (or in a then-like clause after an &&) and therein lies a danger. It might be tempting to write something like this:

```
# Caution: not what you might think!
cd /tmp || echo "cd to /tmp failed." ;  exit
```

The “or” connection tells us that if the cd fails we will execute the echo command, telling the user that the cd failed. But here’s the catch: the exit will happen regardless. Not what you expected, right?
