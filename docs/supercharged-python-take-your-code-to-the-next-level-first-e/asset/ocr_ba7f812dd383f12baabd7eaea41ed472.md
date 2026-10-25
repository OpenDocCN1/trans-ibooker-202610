*list
Replaces list with a series of individual, “unpacked”
values. The use of this operator with *args is explained in
Section 4.8, “Variable-Length Argument Lists.

The first two of these operators (+ and *) involve making
copies of list items. But these are shallow copies. (Section 3.7,
“Shallow Versus Deep Copying,” discusses this issue in greater
detail.) So far, shallow copying has worked fine, but the issue
will rear its head when we discuss multidimensional arrays in
Section 3.18.

Consider the following statements:

Click here to view code image

```
a_list = [1, 3, 5, 0, 2]
b_list = a_list		# Make an alias.
c_list = a_list[:]		# Member-by-member copy
```

After b_list is created, the variable name b_list is just an
alias for a_list. But the third statement in this example
creates a new copy of the data. If a_list is modified later,
c_list retains the original order.

The multiplication operator (*) is particularly useful when
you’re working with large lists. How do you create an array of
size 1,000 and initialize all the elements to zero? Here’s the
most convenient way:

```
big_array = [0] * 1000
```

The test for equality (==) and test for inequality (!=) work on