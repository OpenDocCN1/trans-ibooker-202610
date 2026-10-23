Do the following two snippets of code do exactly the same thing? Assume that temperature already refers to a number.

# Snippet 1:

```
if temperature > 0:
    print('warm')
elif temperature == 0:
    print('zero')
else:
    print('cold')
```

# Snippet 2:

```
if temperature > 0:
    print('warm')
elif temperature == 0:
    print('zero')
print('cold')
```

+   A. Yes
+   B. No

Answer: B. Snippet 2 always prints cold as its final line of output, because print('cold') is not indented! It is not associated with any if statement.

# Solving the Problem

It’s time to solve Winning Team. In this book, I’ll generally present the full code and then discuss it. But as our solution here is longer than those in Chapter 1, I’ve decided in this case to present the code in three pieces before presenting it as a whole.

First, we need to read the input. This requires six calls of input, because we have two teams and three pieces of information for each team. We also need to convert each piece of input to an integer. Here’s the code: