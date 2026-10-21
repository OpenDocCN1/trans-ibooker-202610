![](bbox=[0.14, 0.08, 0.26, 0.15])

# A Subtle Sort of Difference

Within the double square brackets, the < and > operators compare “lexicographically using the current locale,” whereas test (and [ ) compare using simple ASCII ordering.

You will also likely need to escape these operators (like this: if [ $x \> $y ]) when using single brackets, otherwise they will be taken to mean redirection. Why? Because the single bracket, like the test command, is a builtin command not a keyword, so bash sees it as running a command—and you can redirect I/O when running a command. However, when bash sees the double brackets, a keyword, it knows to expect such operators and doesn’t treat them as redirection. Therefore, of the two syntax forms, we much prefer the double-bracket syntax.

Both single- and double-bracket expressions can use an older, more Fortran-like syntax for their numeric comparisons. For example, they use -le for less-than-or-equal-to comparison. Here’s where another difference between the two arises. The arguments to either side of this operator must be simple integers in the single bracket expression. Using double brackets, each operand can be a larger arithmetic expression, though without spaces unless quoted. For example:

```
if [[ $OTHERVAL*10 -le $VAL/5 ]] ; then ...
```

A better choice if you’re doing arithmetic expressions and comparisons is to use the double-parentheses syntax. That gives you the more familiar C/Java/Python-like comparison operators and more freedom regarding spacing:

```
if (( OTHERVAL * 10 <= VAL / 5 )) ; then ...
```

# Style and Readability: Recap

With so many variations to choose from, which if statement style do you choose? We choose the style that best fits the calculation under consideration.

When it is a mathematical expression, we use the double parentheses. As a rule, in bash, double parens indicate arithmetic is going on. The dollar sign indicates that you want the value of the expression returned, otherwise you just get a success/fail result status. But operator-rich bash makes it possible to do similar things using either the double-paren syntax or the let builtin. Since the $ isn’t needed on variables to get their values inside double parentheses, we try to omit them consistently.

For arithmetic expressions, some people may prefer the double parentheses around the expression, consistent with the if statements. However, for others the simple let builtin command reads cleanly and simply. You can live dangerously and skip the double parens by declaring your variables as integers, but we cannot recommend