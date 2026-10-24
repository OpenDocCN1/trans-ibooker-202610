```
curDate.setTime(curDate.getTime() + msPerDay);
```

```
}
}
...
```

The architecture of the samples is designed to be loaded from a database, and that functionality will be used in the examples in Chapter 11. However, to facilitate running the examples, most of the time they will use a mock entity manager that generates random data for the series. In essence, most examples are module-level mesobenchmarks that are suitable for illustrating the performance issues at hand—but we would have an idea of the actual performance of the application only when the full application is run (as in Chapter 11).

One caveat is that numerous examples are therefore dependent on the performance of the random number generator in use. Unlike the microbenchmark example, this is by design, as it allows the illustration of several performance issues in Java. (For that matter, the goal of the examples is to measure the performance of an arbitrary thing, and the performance of the random number generator fits that goal. That is quite different from a microbenchmark, where including the time for generating random numbers would affect the overall calculation.)

The examples also heavily depend on the performance of the BigDecimal class, which is used to store all the data points. This is a standard choice for storing currency data; if the currency data is stored as primitive double objects, rounding of half-pennies and smaller amounts becomes quite problematic. From the perspective of writing examples, that choice is also useful as it allows some “business logic” or lengthy calculation to occur—particularly in calculating the standard deviation of a series of prices. The standard deviation relies on knowing the square root of a BigDecimal number. The standard Java API doesn’t supply such a routine, but the examples use this method:

```
public static BigDecimal sqrtB(BigDecimal bd) {
    BigDecimal initial = bd;
    BigDecimal diff;
    do {
        BigDecimal sDivX = bd.divide(initial, 8, RoundingMode.FLOOR);
        BigDecimal sum = sDivX.add(initial);
        BigDecimal div = sum.divide(TWO, 8, RoundingMode.FLOOR);
        diff = div.subtract(initial).abs();
        diff.setScale(8, RoundingMode.FLOOR);
        initial = div;
    } while (diff.compareTo(error) > 0);
    return initial;
}
```

This is an implementation of the Babylonian method for estimating the square root of a number. It isn’t the most efficient implementation; in particular, the initial guess