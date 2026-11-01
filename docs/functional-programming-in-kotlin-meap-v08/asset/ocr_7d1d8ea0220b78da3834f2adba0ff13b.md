![](bbox=[0.13, 0.05, 0.38, 0.43])

# Figure 6.1 Using the Random class to generate pseudo-random numbers while mutating state

Let’s just take testability as an example. If we want to write a method that makes use of randomness, we need tests to be reproducible. Let’s say we had the following side-effecting method, intended to simulate the rolling of a single six-sided die, which should return a value between 1 and 6, inclusive:

# Listing 6.2 A method simulating the roll of a die with an off-by-one error.

```
fun rollDie(): Int {
    val rng = kotlin.random.Random
    return rng.nextInt(6)
}
```

+   ① Method should return a random number from 1 to 6
+   ② Generator returns a random number from 0 to 5

This method has an off-by-one error. It’s supposed to return a value between 1 and 6, but it actually returns a value from 0 to 5. But even though it doesn’t work properly, five out of six times a test of this method will meet the test specification! And if a test did fail, it would be ideal if we could reliably reproduce the failure before we attempt to fix it.

Note that what’s important here is not this specific example, but the general idea. In this case, the bug is obvious and easy to reproduce. But we can easily imagine a situation where the method is much more complicated and the bug far more subtle. The more complex the program and the