
here are some questions to consider:

- Can you implement a function with the same signature as `map2`, but using `flatMap` and `unit`? How is its meaning different than that of `map2`?
- Can you think of laws relating `join` to the other primitives of the algebra?
- Are there parallel computations that can’t be expressed using this algebra? Can you think of any computations that can’t even be expressed by adding new primitives to the algebra?

## SIDEBAR Recognizing the expressiveness and limitations of an algebra

As you practice more functional programming, one of the skills you'll develop is the ability to recognize *what functions are expressible from an algebra*, and what the limitations of that algebra are. For instance, in the preceding example it may not have been obvious at first that a function like `choice` couldn't be expressed purely in terms of `map`, `map2`, and `unit`. It may also not have been obvious that `choice` was just a special case of `flatMap`. Over time, observations like this will come quicker, and you'll also get better at spotting how to modify your algebra to make some needed combinator expressible. These skills will be helpful for all of your API design work.

Being able to reduce an API to a minimal set of primitive functions is an extremely useful skill. It often happens that primitive combinators encapsulate some tricky logic, and reusing them means we don't have to duplicate our work.

We’ve now completed the design of a library for defining parallel and asynchronous computations in a purely functional way. Although this domain is interesting, the primary goal of this chapter was to give you a window into the process of functional design, a sense of the kind of issues you’re likely to encounter, and ideas on how to handle such issues.

Chapters 4 through 6 continually referred to the principle of *separation of concerns*. Specifically, the idea of separating the *description* of a computation from the *interpreter* that runs it. In this chapter we saw this principle in action—we designed a library that describes parallel computations as values of a data type `Par`, with a separate interpreter called `run` to spawn threads to execute them.

## 7.6 Summary

- Functional API design is an iterative and exploratory process driven by real-world examples.
- A purely functional library that deals with parallelization is a perfect example to demonstrate API design.
- *Data types* and their associated *functions* are born out of exploring domain examples.
- Treating an API as you would an *algebraic equation* leads to following types to a concrete implementation.
- Laws help define constraints on operations, lead to implementation choices and validate properties.
- Combinators can often be generalized to broaden their application across many different applications and scenarios.
- Effective library design separates description of computations from the interpreter that will be responsible for running them.

## Property-based testing

### This chapter covers:

- Understanding the concept of property-based testing
- Model properties by discovering appropriate data types and functions
- Fabricating test data using generators
- Minimizing test case outcomes to give meaningful feedback
- Using properties to affirm laws
- Applying syntactic sugar to improve user experience

In chapter 7 we worked through the design of a functional library for expressing parallel computations. We introduced the idea that an API should form an *algebra*—that is, a collection of data types, functions over these data types, and importantly, *laws* or *properties* that express relationships between these functions. We also hinted at the idea that it might be possible to somehow *validate* these laws automatically. Validation is an important step, as we need to know that the code we write is in conformance with the laws that we have imposed upon our program. It would be of great benefit if we could somehow automate this validation process.

This chapter will take us toward a simple but powerful library for automated *property-based testing*. The general idea of such a library is to decouple the specification of program behavior from the creation of test cases. The programmer focuses on specifying the behavior of a program and giving high-level constraints on the test cases. The framework then automatically generates test cases that satisfy these constraints and runs tests to validate that the program behaves as specified.

Although a library for testing has a very different purpose than a library for parallel computations, surprisingly we’ll discover that they both have very similar combinators. This similarity is something we’ll return to again in part 3.

## 8.1 A brief tour of property-based testing

Property-based testing frameworks are already broadly accepted and used among functional programmers in many different languages such as Haskell, Scala, and even in Kotlin. As an example, let’s look at KotlinTest, a popular testing framework for Kotlin development. It has built-in support for property-based testing in which a property looks something like this.

### Listing 8.1 Demonstration of property-based testing using KotlinTest

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

- ① A generator of lists containing integers between 0 and 100.
- ② A valid property that specifies the behavior of the List.reversed method.
- ③ Check that reversing a list twice gives back the original list.
- ④ Check that the first element becomes the last element after reversal.
- ⑤ Second property that fails under most conditions

Here, intList is not a List<Int> as you might expect, but rather a Gen<List<Int>>, which is something that knows how to generate test data of type List<Int>. We can sample from this generator to produce lists of different lengths, each filled with random numbers between 0 and 100. Generators in a property-based testing library have a rich API. We can combine and compose generators in different ways, reuse them, and so on.

The function forAll creates a property by combining a generator of type Gen<A> with some predicate of type (A) -> Boolean. The property asserts that all values produced by the generator should satisfy this predicate. Like generators, properties should also have a rich API.

Although KotlinTest does not currently support this, we should be able to use operators like and and or to combine multiple properties. The resulting property would hold only if none of the properties could be falsified by any of the generated test cases. Together, these combined properties would form a complete specification of the correct behavior to be validated.

It is well worth noting that the goal of this sort of testing is not necessarily to fully specify program behavior, but rather to give greater confidence in the code. Property-based testing does not replace unit testing, which finds its purpose more in expressing intent and driving design than in validating our confidence in the code.

When we express these properties, KotlinTest will randomly generate `List<Int>` values to try to find a case that falsifies the predicates that we’ve supplied. It generates 100 test cases (of type `List<Int>`) and each list will be checked to see if it satisfies the predicates. Properties can of course fail—the second property should indicate that the predicate tested false for some input, which is then printed to standard out to facilitate further testing or debugging.

## EXERCISE 8.1

To get used to thinking about testing in this way, come up with properties that specify the implementation of a sum: `(List<Int>) -> Int` function. You don’t have to write your properties down as executable KotlinTest code—an informal description is fine. Here are some ideas to get you started:

- Reversing and summing a list should give the same result as summing the original, non-reversed list.
- What should the sum be if all elements of the list are the same value?
- Do any other properties spring to mind?

## EXERCISE 8.2

What properties specify a function that finds the maximum of a `List<Int>`?

Property-based testing libraries often come equipped with other useful features. We’ll talk more about some of these features later, but just to give an idea of what’s possible:

- *Test case minimization* — In the event of a failing test, the framework tries increasingly smaller dataset sizes until it finds the *smallest* dataset that still fails, this being more illuminating for diagnosing failures. For instance, if a property fails for a list of size 10, the framework tries smaller lists and reports the smallest list that fails the test.
- *Exhaustive test case generation* — We call the set of values that could be produced by some `Gen<A>` the *domain*.³⁸ When the domain is small enough (for instance, if it’s all even integers less than 100), we may exhaustively test all its values rather than generate sample values. If the property holds for all values in a domain, we have an actual *proof* rather than just the absence of evidence to the contrary.

KotlinTest is just one framework that provides property-based testing capabilities. And while there’s nothing wrong with it, we’ll derive our own library in this chapter, starting from scratch. Like in chapter 7, this is mostly for pedagogical purposes, but also partly because we should consider no library to be the final word on any subject. There is certainly nothing wrong with using an existing library like KotlinTest, and existing libraries can be a good source of ideas. But even if you decide you like the existing library’s solution, spending an hour or two playing with designs and writing down some type signatures is a great way to learn more about the domain and understand the design trade-offs.
