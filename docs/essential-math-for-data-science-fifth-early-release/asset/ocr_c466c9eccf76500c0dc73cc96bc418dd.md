It is also common to see $n$ represent the number of items in a collection, like the number of records in a dataset. Here is one such example where we iterate a collection of numbers of size $n$, multiply each one by 10, and sum them.

![](bbox=[0.447, 0.206, 0.542, 0.253])

In Example 1-11 we use Python we execute this expression on a collection of 4 numbers. Note that in Python (and most programming languages in general) we typically reference items starting at index 0, while in math we start at index 1. Therefore we shift accordingly in our iteration by starting at 0 in our `range()`.

# Example 1-11. Summation of elements in Python

```
x = [1, 4, 6, 2]
n = len(x)

summation = sum(10*x[i] for i in range(0,n))
print(summation)
```

That is the gist of summation. In a nutshell, a summation $\Sigma$ says “add a bunch of things together” and uses an index $i$ and a maximum value $n$ to express each iteration feeding into the sum. We will see them throughout this book.