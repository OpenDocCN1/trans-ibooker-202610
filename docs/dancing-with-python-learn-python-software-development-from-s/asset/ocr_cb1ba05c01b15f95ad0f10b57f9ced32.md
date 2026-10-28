```
cache = {0:0, 1:1}

# Compute Fibonacci(n - 1) first, thereby calculating all
# Fibonacci values for values less than n - 1

if n - 1 in cache:
    fibonacci_n_minus_1 = cache[n - 1]
else:
    cache[n - 1] = fibonacci_n_minus_1 = cached_fibonacci(n - 1, cache)

# I know we have computed Fibonacci(n - 2)

result = cache[n] = fibonacci_n_minus_1 + cache[n - 2]
return result

for j in range(16):
    print(f"{cached_fibonacci(j)} ", end="")
```

0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610

Note how I use None and the default argument value for cache.

# Exercise 6.22

For n = 0 to 9, compute how many calls Cₙ to cached_fibonacci we must do to calculate Fₙ.

In this cached version, we calculate a value in the Fibonacci sequence once, and do not recurse unless we must. We remove the recursion completely with this iterative version that includes an elegant use of simultaneous assignment:

```
def iterative_fibonacci(n):
    if not isinstance(n, int) or n < 0:
        raise ValueError("Argument must be a non-negative integer.")

    if n < 2:
        return n
```