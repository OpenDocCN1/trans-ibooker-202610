# Creating Concise Exception Tests

When Java 5 annotations were introduced, JUnit[13] was quick
to use them. Overall this was a benefit, but one use in
particular, the convenience of exception tests, led to terse
rather than concise code. Let's understand the issues and
then resolve them using—good guess—lambda expressions.
We'll see here that lambda expressions aren't just another
language feature; they alter the way we think, design, and
even test applications.

Suppose we're driving the design of a class, RodCutter, through
unit tests and we expect a maxProfitmethod to throw an
exception if the argument is zero. Let's look at a few ways we
can write the exception tests for it.

# Attempt 1: Verbose Tests with try and catch

Here's a test for the maxProfit method with try and catch to check
for exceptions.

![](bbox=[0.146, 0.596, 0.882, 0.626])

```
@Test public void verboseExceptionTest() {
rodCutter.setPrices(prices);

try {
rodCutter.maxProfit(0);
fail("Expected exception for zero length");
} catch(RodCutterException ex) {
assertTrue(true);
}
}
```

That's verbose and it may take some effort to understand, but
this code is specific about what's expected to fail: the call to
the maxProfit method.