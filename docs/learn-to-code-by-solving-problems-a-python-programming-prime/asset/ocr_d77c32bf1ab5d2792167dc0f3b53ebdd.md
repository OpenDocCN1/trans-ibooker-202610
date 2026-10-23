```
>>> for value in proportions:
...   value = float(value)
...   type(value)
...
<class 'float'>
<class 'float'>
<class 'float'>
<class 'float'>
```

Four floats! But the list obdurately remains one of strings.

What’s happening here is that we’re not changing the values referred to in the list. We’re changing what the variable value refers to, but that doesn’t change the fact that the list refers to the old string values. To actually change the values that the list references, we need to assign new values at the list’s indices. Here’s how to do it:

```
>>> proportions
['0.2', '0.08', '0.4', '0.32']
>>> for i in range(len(proportions)):
...   proportions[i] = float(proportions[i])
...
>>> proportions
[0.2, 0.08, 0.4, 0.32]
```

The range for loop loops through each index, and an assignment statement changes what’s referred to by that index.

# Solving Most of the Problem

We’re now in good shape to solve the problem minus the catch.

We’ll start with an example to highlight what our code will have to do. Then we’ll move onto the code itself.

# Exploring a Test Case

The input for this problem consists of 10 test cases, but I’ll present only one here. If you type this test case from the keyboard, you’ll see the answer. But the program won’t terminate there, because it’s waiting for the next test case. If you use input redirection with this test case, you’ll again see the answer. But then you’ll get an EOFError. EOF stands for “end of file”; the error is caused by the program trying to read more input than is available. Once your code is working for one test case, you