```
Calling fibonacci(5):
    fibonacci(3) is fetched from cache[3] as: 2
    fibonacci(4) is fetched from cache[4] as: 3
    fibonacci(5) is computed and cached at cache[5]
Result of fibonacci(5) is 5

Calling fibonacci(6):
    fibonacci(4) is fetched from cache[4] as: 3
    fibonacci(5) is fetched from cache[5] as: 5
    fibonacci(6) is computed and cached at cache[6]
Result of fibonacci(6) is 8
```

Each fibonacci(k) method is computed from the cached fibonacci(k-1) and fibonacci(k-2) methods. Fetching the computed values from the cache and summing them is a constant time work. Since we do this work k times, this means that Big O can be expressed as O(n).

Besides *Memoization*, we can use another approach, known as *Tabulation*. More details are available in *Chapter 8, Recursion and Dynamic Programming*.

# Example 12 – looping half of the matrix

What is Big O for the following two snippets of code (a is an array)?

```
```java
// snippet 1                         // snippet 2
for (int i=0;i<a.length;i++){        for (int i=0;i<a.length;i++){
  for (int j=0;j<a.length;j++){        for (int j=i+1;j<a.length;j++){
    System.out.println(a[i]+a[j]);       System.out.println(a[i]+a[j]);
  }                                     }
}                                     }
```
```

7.12 – Code snippets for Big O

These snippets of code are almost identical, except that in the first snippet, j starts from 0, while in the second snippet, it starts from i+1.

We can easily give value to the array size and visualize the runtime of these two snippets of code. For example, let's consider that the array size is 5. The left-hand matrix is the runtime of the first snippet of code, while the right-hand matrix corresponds to the runtime of the second snippet of code: