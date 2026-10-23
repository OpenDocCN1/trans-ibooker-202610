# CONCEPT CHECK

Let’s think about a new version of Farm Seeding where there’s no restriction on the number of cows that care about a pasture. A pasture might be the favorite of four cows, five cows, or even more. We’re still not allowed to give a cow two pastures with the same grass type.

Suppose that we’re solving this new version of the problem and have a test case where a pasture is the favorite of more than three cows. Which of the following is true of that test case?

+   A. It’s *guaranteed* that there’s *no way* to solve it with only four grass types.
+   B. There might be a way to solve it. If there is, it’s *possible* that our original solution (Listing 7-2) *will* do so.
+   C. There might be a way to solve it. If there is, it’s *guaranteed* that our original solution (Listing 7-2) *will* do so.
+   D. There might be a way to solve it. If there is, it’s *guaranteed* that our original solution (Listing 7-2) *won’t* do so.

Answer: B. We can find a test case that is correctly solved by our program, and we can find a test case that can be solved but not by our program. The former eliminates A and D as correct responses; the latter eliminates C as the correct response.

Here’s a test case that is correctly solved by our program:

```
2 4
1 2
1 2
1 2
1 2
```

Each pasture is the favorite of four cows. Nevertheless, we can solve this test case using only two grass types. Try our program, and you should see that it correctly solves this test case.