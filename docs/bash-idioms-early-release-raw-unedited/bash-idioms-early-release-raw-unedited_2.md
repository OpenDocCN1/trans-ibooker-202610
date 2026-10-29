# Chapter 1. A Big “If” Idiom

## A NOTE FOR EARLY RELEASE READERS

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

## The Big “if”

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

## BASH HELP

The Bash help command is great for any builtin command, but `help test` provides especially helpful clues about the test command line `-n`. You might think to check `man bash` but if you do you’ll get a very long and somewhat cryptic “all about bash” page. Don’t get us wrong, there is great information in `man bash`, but `help <thing>` is probably what you really want. If you are not sure if the command in question is a bash builtin, just try it, or use `type -a` to find out.

Admittedly, knowing that `help test` will tell you what `-n` means is tricky, but then, you were smart enough to buy this book, so now you know. Here is another subtle little tidbit, go try it: `help [`. You’re welcome.

## Or ELSE...

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

## More Than One

You might want to do more than one thing in this else clause - and therein lies a danger. It might be tempting to write something like this:

```
#### caution: not what you might think!
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
#### succeed with the cd or bail with a message
cd $DIR || { echo "cd to $DIR failed." ;  exit ; }
```

The braces are bash syntax for a compound command, i.e., grouping statements together. You may have seen something similar using parentheses, but using parentheses executes the statements in a sub-shell. That would incur an overhead we don’t need, and the exit occurring from within a sub-shell wouldn’t accomplish much either.




## 关闭复合命令

bash 的一个怪癖要求关闭复合命令时使用特定的语法。它必须以分号或换行符结束，然后才是闭合大括号。如果使用分号，则在大括号前需要有空白，以便大括号能被识别为保留字（否则它会被误认为是 shell 变量语法中的闭合大括号，例如 ${VAR}）。这就是为什么我们上面的示例以看起来多余的分号结尾的原因：{ echo "..." ; exit ; }。使用换行符，则最后一个分号就不需要了：

```
#### succeed with the cd or bail with a message
cd $DIR || { echo "cd to $DIR failed." ; exit
}
```

但这样可能读起来不够清晰。在左侧边缘看起来放置得很奇怪；用空白缩进会更逻辑地分组，但显得很单薄。

我们建议你保留额外的分号，并且不要忘记它与闭合大括号之间的空格。

#### 再来一次多于一个

如果需要更复杂的逻辑怎么办？多重 AND 和 OR 构造又如何？它们是如何处理的？你认为下面这行代码会做什么？

```
[ -n "$DIR" ] && [ -d "$DIR" ] && cd "$DIR" || exit 2
```

它做了你可能想到的事情——但也许不是你想到的原因。你可能从这个例子中推断出 && 的优先级高于 || 运算符，但事实并非如此。它们只是从左到右分组。bash 的语法规定 && 和 || 运算符具有相同的优先级并且是左关联的。需要说服力吗？

```
$ echo 1 && echo 2 || echo 3
1
2
$
```

但也有：

```
$ echo 1 || echo 2 && echo 3
1
3
$
```

注意它总是评估最左侧的运算符，无论是 AND 还是 OR；决定求值顺序的不是运算符优先级，而是简单的左关联性。

## 风格和可读性：何时使用哪种形式的 If

通过使用 && 和 || 运算符，你可以在不使用那些熟悉的关键字的情况下编写 if/then/else 逻辑。但 bash 确实有 if、then 和 else 作为关键字。那么何时使用它们，何时使用简写？答案取决于可读性。

对于复杂逻辑，使用熟悉的关键字更有意义。但对于简单的测试和检查情况，配合简单的单个操作，使用 && 和 || 运算符的简写非常方便，并且不会分散主逻辑流程的注意力。

## 总结

在本章中，我们查看了 bash 的一个特定习惯用法——“无 if” 的 if 语句。它看起来不像传统的 if/then/else，但可以完全像它一样表现。除非你认识这种写法，否则你阅读的一些脚本可能会仍然晦涩。这也使得它值得使用，作为在执行命令之前检查必要的先决条件是否就位的方式，或在不破坏脚本主逻辑流程的情况下进行简短的错误检查。

现在你已经深入了解了一个 bash 习惯用法，让我们看看其他习惯用法，并真正提升你的 bash 技能。
