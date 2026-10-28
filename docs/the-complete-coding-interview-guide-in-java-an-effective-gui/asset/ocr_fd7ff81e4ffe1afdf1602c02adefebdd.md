```
```java
// snippet 1

int min = Integer.MAX_VALUE;

int max = Integer.MIN_VALUE;

for (int i = 0; i < a.length; i++) {
    if (a[i] < min) {
        min = a[i];
    }
    if (a[i] > max) {
        max = a[i];
    }
}
```
```

```
```java
// snippet 2

int min = Integer.MAX_VALUE;

int max = Integer.MIN_VALUE;

for (int i = 0; i < a.length; i++) {
    if (a[i] < min) {
        min = a[i];
    }
}

for (int i = 0; i < a.length; i++) {
    if (a[i] > max) {
        max = a[i];
    }
}
```
```

# 7.3 – Code Comparison

Now, which one of these two code snippets runs faster?

The first code snippet uses a single loop, but it has two if statements, while the second code snippet uses two loops, but it has one if statement per loop.

Thinking like this opens the door to insanity! Counting the statements can continue at a deeper level. For example, we can continue to count the statements (operations) at the compiler level, or we might want to take into consideration the compiler optimizations. Well, that's not what Big O is about!

> **Important note**
> 
> Big O is not about counting the code statements. Its goal is to express the runtime growth for input sizes and express how the runtime scales. In short, Big O just describes the runtime rate of increase.

Moreover, don't fall into the trap of thinking that because the first snippet has one loop, Big O is O(n), while in the case of the second snippet, because it has two loops, Big O is O(2n). Simply remove 2 from 2n since 2 is a constant!