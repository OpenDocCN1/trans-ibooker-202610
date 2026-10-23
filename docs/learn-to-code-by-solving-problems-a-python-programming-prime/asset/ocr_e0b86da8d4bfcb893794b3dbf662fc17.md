of elifs if you want at most one block of code to execute. An else can help make the code clearer and removes the need to write a catchall Boolean expression. What’s far more important than the precise styling of an if statement is writing correct logic!

# CONCEPT CHECK

What is the value of x after the following code runs?

```
x = 5
if x > 2:
    x = -3
if x > 1:
    x = 1
else:
    x = 3
```

+   A. -3
+   B. 1
+   C. 2
+   D. 3
+   E. 5

Answer: D. Because x > 2 is True, the block of the first if statement executes. The assignment x = -3 makes x refer to -3. Now for the second if statement. Here, x > 1 is False, so the else block runs, and x = 3 makes x refer to 3. I’d suggest changing if x > 1 to elif x > 1 and observing how the behavior of the program changes!

# CONCEPT CHECK