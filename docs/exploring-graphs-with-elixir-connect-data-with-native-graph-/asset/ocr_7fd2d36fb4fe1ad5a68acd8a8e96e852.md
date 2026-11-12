![](bbox=[0.146, 0.086, 0.853, 0.321])

To compute the maximum profit for 5" we need to compute the maximum profit for 4", 3", 2", and 1". Likewise, to compute the maximum profit for 4", we need to compute the maximum profit for the smaller sizes. The solution nicely introduces overlapping recursion; we'll implement this without any optimization first, and then improve on it.

Let's implement the logic for maximum profit.

```
recur/fpij/RodCutter.java

```java
public int maxProfit(final int length) {
    int priceAtLength = length <= prices.size() ? prices.get(length - 1) : 0;

    return Math.max(priceAtLength,
        IntStream.range(1, length)
            .map(i -> maxProfit(i) + maxProfit(length - i))
            .max()
            .orElse(0));
}
```
```

In the maxProfit method we look up the price for a particular length. Then, we recursively find the profit for cuts that add up to the given length and pick the maximum from them. The implementation turns out to be simple. Let's try it for a few lengths.