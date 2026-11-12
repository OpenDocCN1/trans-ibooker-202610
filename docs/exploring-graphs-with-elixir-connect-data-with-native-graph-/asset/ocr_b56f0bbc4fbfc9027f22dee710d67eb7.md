With this step, we gained some and lost some. We reduced the code and removed a few loops; less code is better. But we're still being imperative, mutating variables. Furthermore, if we want to change the logic—say we want to pick the highest-priced stock under $1,000—we have to modify this code. No part of it is reusable. Going back to our three-step version of the code, we could modularize each part into a function for reuse. Rather than combining the steps, if we keep them as distinct steps we can convert them easily from the imperative style to the functional style, as we'll see next.

# ...To the Functional Style

We used three different loops in the imperative version, but the JDK provides specialized functional-style methods for each of those steps. We can easily use those convenience methods and we won't have to manually create any internal iterators to program the logic in the functional style. Let's refactor the code into the functional style; this version is declarative, preserves immutability, is concise, and uses function composition.

```
applying/fpij/PickStockFunctional.java
public static void findHighPriced(final Stream<String> symbols) {
final StockInfo highPriced =
symbols.map(StockUtil::getPrice)
.filter(StockUtil.isPriceLessThan(500))
.reduce(StockUtil::pickHigh)
.get();

System.out.println("High priced under $500 is " + highPriced);
}
```

In the method findHighPriced, we employ method chaining and transform objects. We start with a Stream of symbols and flow into streams of stocks. We first map the symbols into stocks