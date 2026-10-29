
## NOTE

For more than 40 years, *O’Reilly Media* has provided technology and business training, knowledge, and insight to help companies succeed.

Our unique network of experts and innovators share their knowledge and expertise through books, articles, conferences, and our online learning platform. O’Reilly’s online learning platform gives you on-demand access to live training courses, in-depth learning paths, interactive coding environments, and a vast collection of text and video from O’Reilly and 200+ other publishers. For more information, please visit *http://oreilly.com*.

## How to Contact Us

Please address comments and questions concerning this book to the publisher:

O’Reilly Media, Inc.

1005 Gravenstein Highway North

Sebastopol, CA 95472

800-998-9938 (in the United States or Canada)

707-829-0515 (international or local)

707-829-0104 (fax)

We have a web page for this book, where we list errata, examples, and any additional information. You can access this page at *https://learning.oreilly.com/library/view/bash-idioms/9781492094746*.

Email *bookquestions@oreilly.com* to comment or ask technical questions about this book.

For more information about our books, courses, conferences, and news, see our website at *http://www.oreilly.com*.

Find us on Facebook: *http://facebook.com/oreilly*

Follow us on Twitter: *http://twitter.com/oreillymedia*

Watch us on YouTube: *http://www.youtube.com/oreillymedia*

## Acknowledgments

- 1. *https://www.merriam-webster.com/dictionary/idiom* as viewed on 2020-07-01
- 2. If you are new to programming or bash, you may want to start with *Learning the bash Shell* and come back to this book later.
- 3. *https://en.wikipedia.org/wiki/POSIX*
- 4. *https://gitforwindows.org/*
- 5. *https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux*

## Chapter 1. A Big “If” Idiom

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 1st chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the authors at bash-idioms@jpsdomain.org.

To get you started in your understanding of bash idioms we’ll look at a bash construct that allows you to do what you might normally do with an if/then/else construct but with sparser syntax. The idiomatic expression we’ll show you in this chapter has some real advantages - mostly terseness - and some pitfalls to avoid. But if you don’t know bash idioms you might not recognize or realize what is going on.

Take a look at this piece of code:

```
[ -n "$DIR" ] && cd "$DIR"
```

Do you think that looks like an “if” statement? If you’re conversant in bash you’ll recognize that it is functionally the same thing; you’ll understand it as an “if” statement even though the “if” keyword doesn’t appear in the code.

What’s going on?

### The Big “if”

Let’s now look at a similar but simpler example:

```
cd tmp && rm scratchfile
```

This too, is, in effect, an if statement. If the cd command succeeds then (and only then) execute the rm command. The “idiom” here is the use of the double ampersand "&&" (typically read as “and”) to separate the two commands.

Logic or philosophy classes teach the rule: the expression “A AND B” is true if and only if both A and B are each true. Therefore if A is false, there is no need to even consider the value of B. For example, consider “I own a dog AND I own a cat”. If I do not own a dog then this compound expression is false for me, regardless of my cat situation.

Let’s apply this to bash. Remember that the basic function of bash is to execute programs. The first part of this statement has the cd command execute. Similar to the logic of the AND, if this first command fails, bash will not bother to execute the second part, the rm command.

The use of the && is meant to remind you of the AND behavior. bash isn’t actually performing an “AND” operation on these two results. (If this were C/C++ that would be a different story, though the conditional execution is the same.) However, this bash idiom does provide the conditional execution of the second command - it isn’t run if the first command fails.

Let’s go back and look at the original example we gave, namely this expression:

```
[ -n "$DIR" ] && cd "$DIR"
```

Do you think you “get it” now? The first expression tests to see if the length of the value of the variable called DIR is non-zero. If it has a value, that is, if its length is non-zero - and only if it’s non-zero - the cd command will attempt to change into a directory named by the value of DIR.

We could have written this as an explicit IF statement:

```
if [ -n "$DIR" ]; then
  cd "$DIR"
fi
```

To someone less familiar with bash, this latter format is certainly more readable and easily understood. But there isn’t much going on inside the “then” clause, just the cd command, so it seems a bit “bulky” in its syntax. You’ll have to decide which to use based on who your likely readers will be and how likely other commands would be added inside the “then” clause. We’ll offer more opinions on this topic below.

### BASH HELP

The Bash help command is great for any builtin command, but `help test` provides especially helpful clues about the test command line `-n`. You might think to check `man bash` but if you do you’ll get a very long and somewhat cryptic “all about bash” page. Don’t get us wrong, there is great information in `man bash`, but `help <thing>` is probably what you really want. If you are not sure if the command in question is a bash builtin, just try it, or use `type -a` to find out.

Admittedly, knowing that `help test` will tell you what `-n` means is tricky, but then, you were smart enough to buy this book, so now you know. Here is another subtle little tidbit, go try it: `help [`. You’re welcome.

### Or ELSE...

A similar idiom is available using the `||` characters to separate two items in a bash list. Pronounced “or”, the second part will execute only if the first part fails. This is meant to remind you of the logic rule for “OR”, as in: A OR B. The whole expression is true if either A is true or B is true. Put another way, if A is true it doesn’t matter if B is true or false. For example, consider the phrase “I own a dog OR I own a cat”. If I do, in fact, own a dog, then this expression is true for me regardless of my cat situation.

Applying this to bash:

```
[ -z "$DIR" ] || cd "$DIR"
```

Do you think you can explain this one? If the variable is of length zero then it is “true” so there is no need to execute the second half; no cd command will be run. But if the length of $DIR is non-zero, that would be “false”, so only then would we run the cd command.

You might read the line of bash as “Either $DIR is zero length, OR we attempt to cd into that directory”.

To write that as an explicit “if” statement is a bit odd, as there is no “then” action to be taken. The code after the `||` is like the “else” clause:

```
if [ -z "$DIR" ]; then
  :
else
  cd "$DIR"
fi
```

The ":" is a null statement in shell - so it does nothing in that case.

In summary: two commands separated by a `&&` are like an “if” and its “then” clause; two commands separated by a `||` are like an “if” and its “else” clause.

### More Than One

You might want to do more than one thing in this else clause - and therein lies a danger. It might be tempting to write something like this:

```
# caution: not what you might think!
cd "$DIR" || echo "cd to '$DIR' failed." ;  exit
```

The “or” connection tells us that if the cd fails we will execute the echo command, telling the user that the cd failed. But here’s the catch: the exit will happen regardless. Not what you expected, right?

Think of the semicolon as equivalent to a newline and it all becomes much clearer (and more obviously not what you wanted):

```
cd "$DIR" || echo "cd to '$DIR' failed."
exit
```

How can we get the behavior we want? We can group the echo and exit together so that they are, taken together, the clause on the right hand side of the OR, like this:

```
# succeed with the cd or bail with a message
cd $DIR || { echo "cd to $DIR failed." ;  exit ; }
```

The braces are bash syntax for a compound command, i.e., grouping statements together. You may have seen something similar using parentheses, but using parentheses executes the statements in a sub-shell. That would incur an overhead we don’t need, and the exit occurring from within a sub-shell wouldn’t accomplish much either.
