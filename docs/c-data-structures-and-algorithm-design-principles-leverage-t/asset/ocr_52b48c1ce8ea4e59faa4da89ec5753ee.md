The main advantage of implementing a backtracking solution before attempting to use DP is that it requires us to determine the base case(s) and intermediate recursive states of the problem. As we defined earlier in this chapter, a base case is a condition in a recursive function that does not rely on further recursion to produce an answer. For further clarification, consider the problem of calculating the factorial of a number (the factorial of a number, n, is equivalent to n * (n-1) * (n-2) * (n-3) ... * 1). We could code a C++ function that accomplishes this as follows:

```
int Factorial(int n)
{
    // Base case - stop recursing
    if(n == 1)
    {
        return 1;
    }

    // Recurse until base case is reached
    return n * Factorial(n - 1);
}
```

The structure of this recursive function can be illustrated like so:

![](bbox=[0.07, 0.71, 0.94, 0.82])

Figure 8.4: Recursively calculating the Nth factorial

The n = 1 condition is the base case because that is the point at which the solution can be returned without recursing further.