final stage in the pipeline extracts the result from the `success` it receives or the details of the error if it receives a `Failure`.

Let's rewrite the `getNamesOfAirports` method to use this approach, using the yet-to-be-written `Try` interface and `Success`, and `Failure` classes.

```
public static List<Try<String>> getNamesOfAirports(List<String> iataCodes) {
    return iataCodes.stream()
        .map(iataCode -> Try.of(() -> AirportInfo.getNameOfAirport(iataCode)))
        .map(name -> name.map(String::toUpperCase))
        .toList();
}
```

Structurally, this version of `getNamesOfAirports` is the same as the first functional style version we wrote. Whereas the first version ran into compilation errors, in this version we're better equipped to deal with exceptions.

We take a collection of IATA codes and, using the `StreamAPI`, process each element through the pipeline. In the first `map` method call, in the lambda expression passed as an argument, we invoke the `AirportInfo`'s `getNameOfAirport` method, passing the given IATA code. This method, as you know, may return a name of an airport or throw an exception.

We wrap the call to `getNameOfAirport` in another lambda expression and pass that to the yet-to-be- written `Try`'s of method. Since this lambda expression will return a name, it appears that a suitable functional interface to represent this would be a `Supplier<T>`. But the abstract method `get` of that interface doesn't permit any exceptions. Instead, the good old `Callable<T>` interface in `java.util.concurrent` will work great here—other than method names, the main difference between `Callable` and `Supplier` is that the former will permit exceptions to be thrown