```
public RodCutter(final List<Integer> pricesForLengths) {
    prices = pricesForLengths;
}
```

```
//...
```

Let's work with some sample prices for different lengths, starting with 1".

recur/fpij/RodCutter.java

```
final List<Integer> priceValues =
    Arrays.asList(2, 1, 1, 2, 2, 2, 1, 8, 9, 15);

final RodCutter rodCutter = new RodCutter(priceValues);
```

# Plain-Vanilla Recursion

We can solve the problem using a simple recursion. If we're given a 5" rod, we can look up the price for that length. In this example, that would get us $2. We can do better than that— after all, a 4" rod also would fetch $2, so we could cut the rod into two pieces—4" and 1"—to increase profit. Continuing with this approach, we find that the maximum profit for an arbitrary length n is the maximum of the profits from each of the possible 2^(n-1) cuts of that length. That is, max(no cut, cut(1, n - 1), cut(2, n - 2), ...), for a given length n. The following figure is an example of profits from all possible cuts for a 5" rod.