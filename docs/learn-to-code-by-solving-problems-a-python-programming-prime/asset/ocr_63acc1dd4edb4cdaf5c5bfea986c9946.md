What is the output of the following code?

```
s = 'garage'
total = 0

for char in s:
    total = total + s.count(char)

print(total)
```

+   A. 6
+   B. 10
+   C. 12
+   D. 36

Answer: B. For each character in 'garage', we add its count to total. There are two g’s, two a’s, one r, two a’s (again!), two g’s (again!), and 1 e.

# Nesting

The for loop block is one or more statements. Those statements can include one-line statements such as function calls and assignment statements. But they can also include multiline statements such as if statements and loops.

Let’s start with an example of an if statement inside a for loop. Suppose we wanted to output only the uppercase characters from a string. Strings have an isupper method that we can use to determine whether a character is uppercase:

```
>>> 'q'.isupper()
False
>>> 'Q'.isupper()
True
```

We can use isupper in an if statement to control what happens on each iteration of a for loop: