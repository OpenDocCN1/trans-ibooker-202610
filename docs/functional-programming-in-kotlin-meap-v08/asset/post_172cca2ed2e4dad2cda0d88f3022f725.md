
## 8.2 Choosing data types and functions

This section will be another somewhat messy and iterative process of discovering data types and functions for our library. This time around we’re designing a library for property-based testing in order to validate the laws or properties of our programs. As before, this is a chance to peek over the shoulder of someone working through possible scenarios and designs.

The particular path we take and the library we arrive at isn’t necessarily the same as what you would come up with on your own. If property-based testing is unfamiliar to you, even better; this is a chance to explore a new domain and its design space while making your own discoveries about it. If at any point you are feeling inspired or have ideas of your own about how to design a library like this, don’t wait for an exercise to prompt you—put the book down and explore your ideas. You can always come back to this chapter if you run out of ideas or get stuck.

### 8.2.1 Gathering initial snippets for a possible API

With that said, let’s get started. Whenever we begin with library design, we need to define some data types that embody the concepts of our library. With this starting point in mind, what data types should we use for our testing library? What primitives should we define, and what might they mean? What laws should our functions satisfy? As before, we can look at a simple example and “read off” the needed data types and functions and see what we find. For inspiration, let’s look at the KotlinTest example we showed earlier:

```
val intList = Gen.list(Gen.choose(0, 100))

forAll(intList) {
    (it.reversed().reversed() == it) and
    (it.firstOption() == it.reversed().lastOrNone())
}
```

Without knowing anything about the implementation of Gen.choose or Gen.list, we can guess that whatever data type they return (let’s call it Gen, short for generator) must be parametric in some type. That is, Gen.choose(0,100) probably returns a Gen<Int>, and Gen.list is then a function with the signature (Gen<Int>) -> Gen<List<Int>>. But since it doesn’t seem like Gen.list should care about the type of the Gen it receives as input. It would be odd to require separate combinators for creating lists of Int, Double, String and so on, so let’s go ahead and make it polymorphic. We’ll call our method listOf to make the intent clear.

```
fun <A> listOf(a: Gen<A>): Gen<List<A>> = TODO()
```

We can learn many things by looking at this signature. Notice that we’re not specifying the size of the list to generate. For this to be implementable, our generator must therefore either make an assumption, or otherwise be told the size explicitly. Assuming a size seems a bit inflexible, any assumption is unlikely to be appropriate in all contexts. So it seems that generators must be told the size of test cases to generate. We can imagine an API where this is made explicit:

```
fun <A> listOfN(n: Int, a: Gen<A>): Gen<List<A>> = TODO()
```

This would certainly be a useful combinator, but not having to explicitly specify sizes is powerful as well. It means that whatever function runs the tests has the freedom to choose test case sizes, which opens up the possibility of doing the test case minimization we mentioned earlier. If the sizes are always fixed and specified by the programmer, the test runner won’t have this flexibility. Keep this concern in mind as we get further along in our design.

What about the rest of this example? The forAll function looks interesting. We can see that it accepts a Gen<List<Int>> and what looks to be a corresponding predicate, (List<Int>) -> Boolean. But again, it doesn’t seem like forAll should care about the types of the generator and the predicate, as long as they match up. We can express this with the type:

```
fun <A> forAll(a: Gen<A>, f: (A) -> Boolean): Prop = TODO()
```

Here, we’ve simply invented a new type, Prop (short for property) for the result of binding a Gen to a predicate. We might not know the internal representation of Prop or what other functions it supports, but based on our prior discussion in section 8.1, it should be combined with other Prop instances through the use of an and method. Let’s introduce that as a new interface:

```
interface Prop {
    fun and(p: Prop): Prop
}
```

### 8.2.2 Exploring the meaning and API of properties

Now that we have a few fragments of an API, let’s discuss what we want our types and functions to entail. First, let’s consider Prop. We know of functions forAll (for creating a property), and (for composing properties), and will now learn about check. Here we deviate further from KotlinTest’s property design as it doesn’t have such a method in its API. We’ll imagine this to be a method that runs our property and has a side effect of printing to the console. We will expose this as a convenience function on Prop, giving it a return type of Unit for now.

```
interface Prop {
    fun check(): Unit
    fun and(p: Prop): Prop
}
```

This return type does raise an issue in that we can’t chain together multiple checked Props using the and operator. This might remind you of a similar problem that we encountered in chapter 7 when we looked at using Thread and Runnable for parallelism.

Since check has a side effect, the only option for implementing and in this case would be to run check on both Prop instances. So if check prints out a test report we would get two of them,

each printing failures and successes independently of each other. That’s likely not the correct outcome. The problem is not so much that check has a side effect, but more generally that it throws away information by returning Unit.

In order to combine Prop values using combinators like and, we need check (or whatever function “runs” properties) to return some meaningful value. What type should that value have? Well, let’s consider what sort of information we’d expect to get out of checked properties. As a minimum, we need to know whether the property succeeded or failed, so a Boolean return value would do just fine as a first pass. We now have enough to go ahead with implementing the and method.

### EXERCISE 8.3

Assuming the following representation, use check to implement and as a method of Prop.

```
interface Prop {
    fun check(): Boolean
    fun and(p: Prop): Prop = TODO()
}
```

In this representation, Prop is nothing more than a non-strict Boolean. Any of the usual Boolean functions (AND, OR, NOT, XOR, and so on) can easily be defined for Prop. But a Boolean alone is probably insufficient. If a property fails, we might want to know how many tests succeeded first. We might also be interested in what arguments produced the failure. And if a property succeeds, it would be useful to know how many tests it ran. Next, let’s encode this information by returning an Either to indicate success or failure:

```
typealias SuccessCount = Int

interface Prop {
    fun check(): Either<String, SuccessCount>
    fun and(p: Prop): Prop
}
```

For now, we’ve assigned the failure case to be String, but what type should we return on the left side? We don’t know anything about the type of the test cases being generated. Should we add a type parameter to Prop and make it Prop<A> so check could return Either<A, SuccessCount>. Before going too far down this route, let’s ask ourselves whether we really care about the type of the value that caused the property to fail. We don’t really. We would only care about the type if we were going to do further computation with the failure.

### NOTE

We prefer using type aliases instead of simple types like String, Int or Double because we can assign meaningful names to them. This makes our code far easier to comprehend by others who will interact with it.

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

### 8.2.3 Discovering the meaning and API of generators

We determined earlier that a Gen<A> was something that knows how to generate values of type A. How could it go about doing this? Well, it could randomly generate these values. Considering that we devoted all of chapter 6 to this topic, it would seem like we’re missing a trick if we don’t use what we learned in that chapter! Thinking back to our example, we provided an interface for a purely functional random number generator, RNG. We then showed how to make it convenient to combine computations that make use of it. If we recall the definition of State, we can simply make Gen a type that wraps a State transition over a random number generator.
