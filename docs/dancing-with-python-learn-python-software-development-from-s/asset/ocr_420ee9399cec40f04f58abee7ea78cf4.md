If that doesn’t convince you, imagine if I asked you to count to 200. Here is how we do it in C++:

```
int n = 1;

while( n < 201 ) {
    n++;
}
```

# Exercise 1.6

Create a similar while-loop flowchart for counting backward from 100 to 1.

That loop was a while-loop, but this is an until-loop:

+   1. Close your eyes
+   2. Set count to 1
+   3. Increment count by 1 until count equals 10
+   4. Open your eyes

Many languages do not have until-loops, but VBA does:

```
n = 1
Do Until n>200
    n = n + 1
Loop
```

We saw earlier that the code within a function’s definition is its body. The repeated code in a loop is also called its body.