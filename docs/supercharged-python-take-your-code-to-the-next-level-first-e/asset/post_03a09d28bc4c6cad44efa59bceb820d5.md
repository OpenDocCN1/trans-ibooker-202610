

"a", "b" "c", or "d" in the next position.

```
[abcd]
```

Likewise, the following expression says that either a space or
a minus sign (-) can be matched—which is what we want in this
case:

```
[ -]
```

In this context, the square brackets are the only special
characters; the two characters inside are literal and at most one
of them will be matched. The minus sign often has a special
meaning within square brackets, but not when it appears in the
very front or end of the characters inside the brackets.
Here’s the full regular expression we need:

Click here to view code image

```
pattern = r'\d\d\d[ -]\d\d\d[ -]\d\d\d\d$'
```

Now, putting everything together with the refined pattern
we’ve come up with in this section, here’s the complete
example:

Click here to view code image

```
import re
pattern = r'\d\d\d[ -]\d\d\d[ -]\d\d\d\d$'
s = input('Enter tel. number: ')
if re.match(pattern, s):
    print('Number accepted.')
else:
    print('Incorrect format.')
```

To review, here's what the Python regular-expression
evaluator does, given this pattern.

+   - It attempts to match three digits: \\d\\d\\d.
+   - It then reads the character set [ -] and attempts to match either a
+     space or a minus sign, but not both—that is, only one of these two
+     characters will be matched here.
+   - It attempts to match three more digits: \\d\\d\\d.
+   - Again, it attempts to match a space or a minus sign.
+   - It attempts to match four more digits : \\d\\d\\d\\d.
+   - It must match an end-of-string, $. This means there cannot be any
+     more input in the target string after these last four digits are
+     matched.

Another way to enforce an exact match, so that no trailing
data is permitted, is to use the `re.fullmatch` method instead
of `re.match`. You could use the following statements to match
the telephone-number pattern; the use of `fullmatch` makes
the end-of-string character unnecessary in this case.

Click here to view code image

```
import re

pattern = r'\d\d\d[ -]\d\d\d[ -]\d\d\d\d'

s = input('Enter tel. number: ')
if re.fullmatch(pattern, s):
    print('Number accepted.')
else:
    print('Incorrect format.')
```

So far, this chapter has only scratched the surface of what regular-expression syntax can do. Section 6.5 explains the syntax in greater detail. But in mastering this syntax, there are several principles to keep in mind.

+   - A number of characters have special meaning when placed in a regular-expression pattern. It's a good idea to become familiar with all of them. These include most punctuation characters, such as + and *.
+   - Any characters that do not have special meaning to the Python regular-expression interpreter are considered literal characters. The regular-expression interpreter attempts to match these exactly.
+   - The backslash can be used to “escape” special characters, making them into literal characters. The backslash can also add special meaning to certain ordinary characters—for example, causing \d to mean “any digit” rather than a “d”.

Admittedly, this might be a little confusing at first. If a character (such as *) is special to begin with, escaping it (preceding it with a backslash) takes away that special meaning. But in other cases, escaping a character _gives_ it special meaning.

Yes, both those things are true! But if you look at enough examples, it should make sense.

Here’s a short program that tests for the validity of Social Security numbers. It’s similar, but not identical, to that for checking the format of telephone numbers. This pattern looks for three digits, a minus sign, two digits, another minus sign, and then four digits.

Click here to view code image

```
import re

pattern = r'\d\d\d-\d\d-\d\d\d\d$'

s = input('Enter SSN: ')
if re.match(pattern, s):
    print('Number accepted.')
else:
    print('Incorrect format.')
```

## 6.4 HOW REGULAR EXPRESSIONS WORK: COMPILING VERSUS RUNNING

Regular expressions can seem like magic. But the implementation is a standard, if a relatively advanced, topic in computer science. The processing of regular expressions takes two major steps.

+   - A regular expression pattern is analyzed and then compiled into a series of data structures collectively called a _state machine_.
+   - The actual process of matching is considered “run time” for the regular-expression evaluator, as opposed to “compile time.” During run time, the program traverses the state machine as it looks for a match.

Unless you’re going to implement a regular-expression package yourself, it’s not necessary to understand how to create these state machines, only what they do. But it’s important to understand this dichotomy between compile time and runtime.

Let’s take another simple example. Just as the modifier + means “Match one or more instances of the previous expression,” the modifier * means “Match zero or more instances of the previous expression.” So consider this:

```
ca*b
```

This expression matches “cb” as well as “cab”, “caab”, “caaab”, and so on. When this regular expression is compiled, it produces the state machine shown in Figure 6.2.

![](img/6e45221a8147f2442c653a3d4a866570_367_0.png)

The following list describes how the program traverses this state machine to find a match at run time. Position 1 is the starting point.

+   - A character is read. If it’s a “c”, the machine goes to state 2. Reading any other character causes failure.
+   - From state 2, either an “a” or a “b” can be read. If an “a” is read, the machine stays in state 2. It can do this any number of times. If a “b” is read, the machine transitions to state 3. Reading any other character causes failure.
+   - If the machine reaches state 3, it is finished, and success is reported.

This state machine illustrates some basic principles, simple though it is. In particular, a state machine has to be compiled and then later traversed at run time.

> > **Note**
> >
> > The state machine diagrams in this chapter assume DFAs (deterministic finite automata), whereas Python actually uses NFAs (nondeterministic finite automata). This makes no difference to you unless you’re implementing a regular-expression evaluator, something you’ll likely never need to do.
> > So if that’s the case, you can ignore the difference between DFAs and NFAs! You’re welcome.

Here’s what you need to know: If you’re going to use the same regular-expression pattern multiple times, it’s a good idea to compile that pattern into a regular-expression object and then use that object repeatedly. The regex package provides a method for this purpose called **compile**.

![](img/6e45221a8147f2442c653a3d4a866570_368_0.png)

Click here to view code image

```
regex_object_name = re.compile(pattern)
```

Here’s a full example using the **compile** function to create a regular expression object called reg1.

Click here to view code image

```
import re

reg1 = re.compile(r'ca*b$') # Compile the pattern!

def test_item(s):
    if re.match(reg1, s):
        print(s, 'is a match.')
    else:
        print(s, 'is not a match!')

test_item('caab')
test_item('caaxxb')
```

This little program prints the following:

```
caab is a match.
caaxxb is not a match!
```

You could perform these tasks without precompiling a regular-expression object. However, compiling can save execution time if you’re going to use the same pattern more than once. Otherwise, Python may have to rebuild a state machine multiple times when it could have been built only once.

As a point of comparison, Figure 6.3 shows a state machine that implements the plus-sign (+), which means “one or more”

rather than “zero or more.”

![](img/6e45221a8147f2442c653a3d4a866570_370_0.png)

**Figure 6.3.** State machine for `ca+b`

Given this pattern, “cb” is not a successful match, but “cab”, “caab”, and “caaab” are. This state machine requires the reading of at least one “a”. After that, matching further “a” characters is optional, but it can match as many instances of “a” in a row as it finds.

Another basic operator is the _alteration_ operator (`|`), which means “either-or.”

The following pattern matches an expression on either side of the bar. So what exactly do you think the following means?

```
ax|yz
```

The alteration operator, `|`, has about the lowest precedence of any part of the syntax. Therefore, this expression matches “ax” and “yz”, but not “axyz”. If no parentheses are used, the expression is evaluated as if written this way:

```
(ax) | (yz)
```

