
## Example 19 – using n notation with caution

What is Big O for the following two snippets of code?

```cpp
int multiply(int x, int y) {
    int result = 1;

    for (int i=1; i<=y; i++) {
        result *= x;
    }

    return result;
}

int powerxy(int x, int y) {
    if (y < 0) {
        return 0;
    } else if (y == 0) {
        return 1;
    } else {
        return x*powerxy(x, y-1);
    }
}
```

7.15 – Code snippets

The first snippet (on the left-hand side) does constant work for y times. The x input doesn't affect the runtime rate of increase, and so Big O can be expressed as O(y). Pay attention to the fact that we don't say O(n) since n can be confused with x as well.

The second snippet (on the right-hand side) recursively traverses y-1, y-2, ..., 0. Each y input is traversed a single time, so Big O can be expressed as O(y). Again, the x input doesn't affect the runtime rate of increase. Moreover, we avoid saying O(n) since there is more than one input and O(n) will create confusion.

## Example 20 – the sum and count

What is Big O for the following snippet of code (x and y are positive)?

```cpp
int div(int x, int y) {
    int count = 0;
    int sum = y;
    while (sum <= x) {
        sum += y;
        count++;
    }

    return count;
}
```

Let's give values to x and y and watch the count variable, which counts the number of iterations. Consider that x=10 and y=2. For this scenario, count will be 5 (10/2 = 5). Following the same logic, we have x=14, y=4, count=3 (14/4 = 3.5), or x=22, y=3, count=7 (22/3 = 7.3). We can notice that in the worst-case scenario, count is x/y, and so Big O can be expressed as O(x/y).

## Example 21 – the number of iteration counts in Big O

The following snippet of code tries to guess the square root of a number. What is Big O?

```c
int sqrt(int n) {
    for (int guess = 1; guess * guess <= n; guess++) {
        if (guess * guess == n) {
            return guess;
        }
    }

    return -1;
}
```

Let's consider that the number (n) is a perfect square root, such as 144, and we already know that sqrt(144) = 12. Since the guess variable starts from 1 and stops at guess*guess <= n, with step 1, it is quite simple to compute that guess will take the values 1, 2, 3, ..., 12. When guess is 12, we have 12*12 = 144, and the loop stops. So, we had 12 iterations, which is exactly sqrt(144).

We follow the same logic for a non-perfect square root. Let's consider that n is 15. This time, guess will take the values 1, 2, and 3. When guess=4, we have 4*4 > 15 and the loop stops. The returned value is -1. So, we had 3 iterations.

In conclusion, we have sqrt(n) iterations, so Big O can be expressed as O(sqrt(n)).

## Example 22 – digits

The following snippet of code sums up the digits of an integer. What is Big O?

```c
int sumDigits(int n) {
    int result = 0;
    while (n > 0) {
        result += n % 10;
        n /= 10;
```

## Technical requirements

You will find all the code presented in this chapter on GitHub at `https://github.com/PacktPublishing/The-Complete-Coding-Interview-Guide-in-Java/tree/master/Chapter08`.

## Recursion in a nutshell

A method that calls itself directly/indirectly is called recursion. This method is known as a recursive method. The famous Fibonacci numbers problem can be implemented recursively, as follows:

```java
int fibonacci(int k) {

    // base case
    if (k <= 1) {
        return k;
    }

    // recursive call
    return fibonacci(k - 2) + fibonacci(k - 1);
}
```

There are two important parts in this code:

- **Base case:** Returns a value without subsequent recursive calls. For special input(s), the function can be evaluated without recursion.
- **Recursive call:** Since the `fibonacci()` method calls itself, we have a recursive method.

## Recognizing a recursive problem

Before we try to solve a problem via a recursive algorithm, we must recognize it as a good candidate for such an algorithm. Most of the recursive problems used in interviews are famous, so we recognize them by name. For example, problems such as Fibonacci numbers, summing a list of numbers, greatest common divisor, the factorial of a number, recursive Binary Search, reversing a string, and so on are well-known recursive problems.

But what do all these problems have in common? Once we know the answer to this question, we will be able to recognize other recursive problems as well. The answer is quite simple: all these problems can be built off of sub-problems. In other words, we say that we can express the value returned by a method in terms of other values returned by that method.

**Important Note**

When a problem can be built off sub-problems, it is a good candidate for being solved recursively. Typically, such problems include the words list top/last n ..., compute the nᵗʰ or all..., count/find all solutions that ..., generate all cases that ..., and so on. In order to compute the nᵗʰ..., we must compute nᵗʰ⁻¹, nᵗʰ⁻², and so on so that we can divide the problem into sub-problems. In other words, computing f(n) requires computing f(n-1), f(n-2), and so on.

Practice is the keyword in recognizing and solving recursive problems. Solving a lot of recursive problems will help you recognize them just as easily as you blink.

Next, we'll highlight the main aspects of Dynamic Programming and learn how to optimize plain recursion via Dynamic Programming.

## Dynamic Programming in a nutshell

When we talk about optimizing recursion, we talk about Dynamic Programming. This means that solving recursive problems can be done using plain recursive algorithms or Dynamic Programming.

Now, let's apply Dynamic Programming to the Fibonacci numbers, starting with the plain recursive algorithm:

```java
int fibonacci(int k) {
    if (k <= 1) {
        return k;
    }

    return fibonacci(k - 2) + fibonacci(k - 1);
}
```

Memoization is a technique that's used to remove duplicate work in a method. It guarantees that a method is called for the same input only once. To achieve this, Memoization caches the results of the given inputs. This means that, when the method should be called to compute an input that has already been computed, Memoization will avoid this call by returning the result from the cache.

The following code uses Memoization to optimize the plain recursive algorithm for the Fibonacci numbers (the cache is represented by the cache array):

```java
int fibonacci(int k) {
    return fibonacci(k, new int[k + 1]);
}

int fibonacci(int k, int[] cache) {
    if (k <= 1) {
        return k;
    } else if (cache[k] > 0) {
        return cache[k];
    }

    cache[k] = fibonacci(k - 2, cache)
        + fibonacci(k - 1, cache);

    return cache[k];
}
```

If we redraw the tree of calls from the preceding code, then we obtain the following diagram:

![](img/6709a71136a6b327c831c6c15aff2f6a_207_0.png)

Figure 8.3 – Tree of calls (Memoization)

```java
return k;
}

int first = 1;
int second = 0;
int result = 0;

for (int i = 1; i < k; i++) {
    result = first + second;
    second = first;
    first = result;
}

return result;
}
```

The runtime of this algorithm is still O(n), but the space complexity was brought down from O(n) to O(1). So, to recap the Fibonacci numbers algorithms, we have that the following:

- The **plain recursion** algorithm has a runtime of O(2ⁿ) and a space complexity of O(n).
- The **Memoization recursion** algorithm has a runtime of O(n) and a space complexity of O(n).
- The **Tabulation** algorithm has a runtime of O(n) and a space complexity of O(1).

Now, it's time to practice some coding challenges.

## Coding challenges

In the following 15 coding challenges, we will exploit recursion and Dynamic Programming. These problems have been carefully crafted to help you understand and cover a wide range of problems from this category. By the end of this coding challenge session, you should be able to recognize and solve recursive problems in the context of an interview.
