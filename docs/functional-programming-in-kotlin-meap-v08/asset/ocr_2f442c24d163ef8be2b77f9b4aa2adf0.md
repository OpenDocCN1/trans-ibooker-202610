# 8.1 A brief tour of property-based testing

Property-based testing frameworks are already broadly accepted and used among functional programmers in many different languages such as Haskell, Scala, and even in Kotlin. As an example, let’s look at KotlinTest, a popular testing framework for Kotlin development. It has built-in support for property-based testing in which a property looks something like this.

# Listing 8.1 Demonstration of property-based testing using KotlinTest

```
val intList = Gen.list(Gen.choose(0, 100)) ①

forAll(intList) { ②
    (it.reversed().reversed() == it) and ③
    (it.firstOption() == it.reversed().lastOrNone()) ④
}

forAll(intList) { ⑤
    it.reversed() == it
}
```

+   ① A generator of lists containing integers between 0 and 100.
+   ② A valid property that specifies the behavior of the List.reversed method.
+   ③ Check that reversing a list twice gives back the original list.
+   ④ Check that the first element becomes the last element after reversal.
+   ⑤ Second property that fails under most conditions

Here, intList is not a List<Int> as you might expect, but rather a Gen<List<Int>>, which is something that knows how to generate test data of type List<Int>. We can sample from this generator to produce lists of different lengths, each filled with random numbers between 0 and 100. Generators in a property-based testing library have a rich API. We can combine and compose generators in different ways, reuse them, and so on.

The function forAll creates a property by combining a generator of type Gen<A> with some predicate of type (A) -> Boolean. The property asserts that all values produced by the generator should satisfy this predicate. Like generators, properties should also have a rich API.

Although KotlinTest does not currently support this, we should be able to use operators like and and or to combine multiple properties. The resulting property would hold only if none of the properties could be falsified by any of the generated test cases. Together, these combined properties would form a complete specification of the correct behavior to be validated.

It is well worth noting that the goal of this sort of testing is not necessarily to fully specify program behavior, but rather to give greater confidence in the code. Property-based testing does not replace unit testing, which finds its purpose more in expressing intent and driving design than in validating our confidence in the code.