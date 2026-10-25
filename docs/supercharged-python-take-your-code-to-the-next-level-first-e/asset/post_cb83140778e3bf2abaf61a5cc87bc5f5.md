

![](img/6e45221a8147f2442c653a3d4a866570_126_0.png)

Click here to view code image

```
str.lower() # Produce all-lowercase string
str.upper() # Produce all-uppercase string
str.title() # 'foo foo'.title() => 'Foo Foo'
str.swapcase() # Upper to lower, and vice versa
```

The effects of the **lower** and **upper** methods are straightforward. The first converts each uppercase letter in a string to a lowercase letter; the second does the converse, converting each lowercase letter to an uppercase letter. Nonletter characters are not altered but kept in the string as is.

The result, after conversion, is then returned as a new string. The original string data, being immutable, isn’t changed “in place.” But the following statements do what you’d expect.

Click here to view code image

```
my_str = "I'm Henry VIII, I am!"
new_str = my_str.upper()
my_str = new_str
```

The last two steps can be efficiently merged:

```
my_str = my_str.upper()
```

If you then print `my_str`, you get the following:

```
I'M HENRY VIII, I AM!
```

The **swapcase** method is used only rarely. The string it produces has an uppercase letter where the source string had a lowercase letter, and vice versa. For example:

```
my_str = my_str.swapcase()
print(my_str)
```

This prints the following:

```
i'm hENRY viii, i AM!
```

## 2.11 SEARCH-AND-REPLACE METHODS

The search-and-replace methods are among the most useful of the `str` class methods. In this section, we first look at **startswith** and **endswith**, and then present the other search-and-replace functions.

![](img/6e45221a8147f2442c653a3d4a866570_127_0.png)

Click here to view code image

```
str.startswith(substr) # Return True if prefix found.
str.endswith(substr) # Return True if suffix found.
```

One of the authors wrote an earlier book, _Python Without Fear_ (Addison-Wesley, 2018), which features a program that converts Roman numerals to decimal. It has to check for certain combinations of letters at the beginning of the input string—starting with any number of Roman numeral Ms.

Click here to view code image

```
while romstr.startswith('M'):
    amt += 1000    # Add 1,000 to running total.
    romstr = romstr[1:]    # Strip off first character.
```

The `endswith` method, conversely, looks for the presence of a target substring as the suffix. For example:

Click here to view code image

```
me_str = 'John Bennett, PhD'
is_doc = me_str.endswith('PhD')
```

These methods, `startswith` and `endswith`, can be used on an empty string without raising an error. If the substring is empty, the return value is always **True**.

Now let’s look at other search-and-replace methods of Python strings.

![](img/6e45221a8147f2442c653a3d4a866570_128_0.png)

Click here to view code image

```
str.count(substr [, beg [, end]])
str.find(substr [, beg [, end]])
str.index()     # Like find, but raises exception
str.rfind()     # Like find, but starts from end
str.replace(old,new [, count])# count is optional; limits no. of replacements
```

In this syntax, the brackets are not intended literally but represent optional items.

The **count** method reports the number of occurrences of a target substring. Here’s how it works.

Click here to view code image

```
frank_str = 'doo be doo be doo...'
n = frank_str.count('doo')
print(n)    # Print 3.
```

You can optionally use the _start_ and _end_ arguments with this same method call.

Click here to view code image

```
print(frank_str.count('doo', 1))    # Print 2
print(frank_str.count('doo', 1, 10))    # Print 1
```

A _start_ argument of 1 specifies that counting begins with the second character. If _start_ and _end_ are both used, then counting happens over a target string beginning with _start_ position up to but not including the `end` position. These arguments are zero-based indexes, as usual.

If either or both of the arguments (`begin`, `end`) are out of range, the `count` method does not raise an exception but works on as many characters as it can.

Similar rules apply to the **find** method. A simple call to this method finds the first occurrence of the substring argument and returns the nonnegative index of that instance; it returns –1 if the substring isn’t found.

Click here to view code image

```
frank_str = 'doo be doo be doo...'

print(frank_str.find('doo'))   # Print 0
print(frank_str.find('doob'))  # Print -1
```

If you want to find the positions of all occurrences of a substring, you can call the **find** method in a loop, as in the following example.

Click here to view code image

```
frank_str = 'doo be doo be doo...'
n = -1
while True:
    n = frank_str.find('doo', n + 1)
    if n == -1:
        break
    print(n, end=' ')
```

This example prints every index at which an instance of 'doo' can be found.

This example works by taking advantage of the _start_ argument. After each successful call to the **find** method, the initial searching position, n, is set to the previous successful find index and then is adjusted upward by 1. This guarantees that the next call to the **find** method must look for a new instance of the substring.

If the **find** operation fails to find any occurrences, it returns a value of –1.

The **index** and **rfind** methods are almost identical to the **find** method, with a few differences. The **index** function does not return –1 when it fails to find an occurrence of the substring. Instead it raises a **ValueError** exception.

The **rfind** method searches for the last occurrence of the substring argument. By default, this method starts at the end and searches to the left. However, this does not mean it looks for a reverse of the substring. Instead, it searches for a regular copy of the substring, and it returns the starting index number of the last occurrence—that is, where the last occurrence starts.

Click here to view code image

```
frank_str = 'doo be doo be doo...'
print(frank_str.rfind('doo'))  # Prints 14.
```

The example prints 14 because the rightmost occurrence of 'doo' starts in zero-based position 14.

Finally, the **replace** method replaces each and every occurrence of an old substring with a new substring. This method, as usual, produces the resulting string, because it cannot change the original string in place.

For example, let’s say we have a set of book titles but want to change the spelling of the word “Grey” to “Gray.” Here’s an example:

Click here to view code image

```
title = '25 Hues of Grey'
new_title = title.replace('Grey', 'Gray')
```

Printing new_title produces this:

```
25 Hues of Gray
```

The next example illustrates how **replace** works on multiple occurrences of the same substring.

Click here to view code image

```
title = 'Greyer Into Grey'
new_title = title.replace('Grey', 'Gray')
```

The new string is now

```
Grayer Into Gray
```

## 2.12 BREAKING UP INPUT USING “SPLIT”

One of the most common programming tasks when dealing with character input is *tokenizing*—breaking down a line of input into individual words, phrases, and numbers. Python’s **split** method provides an easy and convenient way to perform this task.

![](img/6e45221a8147f2442c653a3d4a866570_133_0.png)

Click here to view code image

```
`input_str.split(delim_string=None)`
```

The call to this method returns a list of substrings taken from *input_string*. The *delim_string* specifies a string that serves as the delimiter; this is a substring used to separate one token from another.

If *delim_string* is omitted or is **None**, then the behavior of **split** is to, in effect, use any sequence of one or more whitespace characters (spaces, tabs, and newlines) to distinguish one token from the next.

For example, the **split** method—using the default delimiter of a space—can be used to break up a string containing several names.

Click here to view code image

```
`stooge_list = 'Moe Larry Curly Shemp'.split()`
```

The resulting list, if printed, is as follows:

