it the Memoizer. It doesn't yet exist, but we'll pretend it does and write the code to use it. Let's refactor the maxProfit method to use a static method, callMemoized, of the Memoizer class.

```
public int maxProfit(final int length) {
    return callMemoized(this::computeMaxProfit, length);
}
```

```
private int computeMaxProfit(
    Function<Integer, Integer> memoizedFunction, int length) {
    int priceAtLength = length <= prices.size() ? prices.get(length - 1) : 0;
    return Math.max(priceAtLength, IntStream.range(1, length)
        .map(i -> memoizedFunction.apply(i) + memoizedFunction.apply(length - i))
        .max()
        .orElse(0));
}
```

Let's look at the crux of the design before we dig into the code. We create a function, computeMaxProfit, and memoize it. The memoized version will look up values before making a call to the actual implementation. Let's discuss how we achieve this.

In the maxProfit method, we call the (yet-to-be-implemented) Memoizer's callMemoized method. To this function we pass a method reference to the computeMaxProfit method as the first argument and the rod length that we're computing the max profit for as the second argument.

The computeMaxProfit method takes two parameters—the first is a reference to the memoized version of the function and the second is the length for which we want to compute the max profit. Within the computeMaxProfit method, we perform our task, and when it's time for recursion, we route the call to memoizedFunction, the memoized function reference. This will return quickly if the value has been cached or memoized.