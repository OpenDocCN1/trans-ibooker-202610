Most likely we’re just going to end up printing it to the screen for inspection by whoever runs the tests. After all, the goal here is to find bugs and to indicate test cases that triggered them so they can be fixed. As a general rule, we shouldn’t use String to represent data that we want to compute with. But for values that we’re just going to show to human beings, a String is absolutely appropriate. This suggests that we can get away with the following representation for Prop:

```
typealias SuccessCount = Int
typealias FailedCase = String

interface Prop {
    fun check(): Either<Pair<FailedCase, SuccessCount>, SuccessCount>
    fun and(p: Prop): Prop
}
```

In the case of failure, check returns a Left(Pair(s,n)), where s is some String that represents the value that caused the property to fail, and n is the number of cases that succeeded before the failure occurred. Conversely, a success would be a Right(n) where n represents the total number of cases succeeded.

For now, that takes care of the return value of check, but what about its arguments? Right now, the check method takes none. Is this sufficient? Since check is a method on Prop, we can think about what information is available to it at the time of its creation. In particular, let’s take another look at forAll:

```
fun <A> forAll(a: Gen<A>, f: (A) -> Boolean): Prop = TODO()
```

Without knowing more about the representation of Gen, it is hard to say whether there is enough information here to be able to generate values of type A. Why is this important? We will need this information to implement check. So for now, we’ll take a step back and turn our attention to Gen to get a better idea of what it means, and what its dependencies might be.

# 8.2.3 Discovering the meaning and API of generators

We determined earlier that a Gen<A> was something that knows how to generate values of type A. How could it go about doing this? Well, it could randomly generate these values. Considering that we devoted all of chapter 6 to this topic, it would seem like we’re missing a trick if we don’t use what we learned in that chapter! Thinking back to our example, we provided an interface for a purely functional random number generator, RNG. We then showed how to make it convenient to combine computations that make use of it. If we recall the definition of State, we can simply make Gen a type that wraps a State transition over a random number generator.