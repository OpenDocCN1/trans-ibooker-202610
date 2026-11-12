Otherwise, it will recursively route the call to the computeMaxProfit method to compute for that length. We'll get a full picture of how this happens when we see the callMemoized method.

The missing piece of the puzzle is how the memoized function is created from the argument passed to callMemoized method. Let's look at the Memoizer class's implementation to get a good understanding of that.

```
```java
recur/fpij/Memoizer.java

public class Memoizer {
    public static <T, R> R callMemoized(
        final BiFunction<Function<T,R>, T, R> functionToMemoize, final T input) {
        Function<T, R> memoizedFunction = new Function<T, R>() {
            private final Map<T, R> store = new HashMap<>();

            public R apply(final T input) {
                if(!store.containsKey(input)) {
                    store.put(input, functionToMemoize.apply(this, input));
                }

                return store.get(input);
            }
        };

        return memoizedFunction.apply(input);
    }
}
```
```

The Memoizer has just one short function. In callMemoized we create an implementation of Function in which we check to see if the solution for a given input is already present in the store HashMap. If a value is present for the given input, we return it;
otherwise, we compute the value for the given input, store it
in the store HashMap, and return the computed value.