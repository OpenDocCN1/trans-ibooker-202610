# 8.2 Choosing data types and functions

This section will be another somewhat messy and iterative process of discovering data types and functions for our library. This time around we’re designing a library for property-based testing in order to validate the laws or properties of our programs. As before, this is a chance to peek over the shoulder of someone working through possible scenarios and designs.

The particular path we take and the library we arrive at isn’t necessarily the same as what you would come up with on your own. If property-based testing is unfamiliar to you, even better; this is a chance to explore a new domain and its design space while making your own discoveries about it. If at any point you are feeling inspired or have ideas of your own about how to design a library like this, don’t wait for an exercise to prompt you—put the book down and explore your ideas. You can always come back to this chapter if you run out of ideas or get stuck.

# 8.2.1 Gathering initial snippets for a possible API

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
fun <A> listOf(a: Gen<A>): List<Gen<A>> = TODO()
```

We can learn many things by looking at this signature. Notice that we’re not specifying the size of the list to generate. For this to be implementable, our generator must therefore either make an assumption, or otherwise be told the size explicitly. Assuming a size seems a bit inflexible, any assumption is unlikely to be appropriate in all contexts. So it seems that generators must be told the size of test cases to generate. We can imagine an API where this is made explicit: