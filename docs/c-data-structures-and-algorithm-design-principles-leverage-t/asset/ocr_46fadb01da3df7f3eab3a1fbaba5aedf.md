```
```text
int result = Fibonacci(n - 1) + Fibonacci(n - 2);
memo[n] = result;

return result;
}
```
```

The recursion tree now looks like this:

![](bbox=[0.241, 0.278, 0.713, 0.628])

Figure 8.2: Computing the nth term in the Fibonacci sequence using cached solutions

By doing this, we have eliminated quite a bit of redundant work. This technique of recursively caching solutions in a top-down manner is known as **memoization**, and can essentially be employed for any DP problem, assuming the following are true:

+   1. You can devise a caching scheme that exploits the similarity of different states while preserving their uniqueness.
+   2. You can accumulate the solutions for the requisite subproblems before exceeding the available stack space.