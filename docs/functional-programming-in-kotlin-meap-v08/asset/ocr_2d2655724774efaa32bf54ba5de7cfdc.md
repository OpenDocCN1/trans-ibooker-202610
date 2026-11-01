When we express these properties, KotlinTest will randomly generate `List<Int>` values to try to find a case that falsifies the predicates that we’ve supplied. It generates 100 test cases (of type `List<Int>`) and each list will be checked to see if it satisfies the predicates. Properties can of course fail—the second property should indicate that the predicate tested false for some input, which is then printed to standard out to facilitate further testing or debugging.

# EXERCISE 8.1

To get used to thinking about testing in this way, come up with properties that specify the implementation of a sum: `(List<Int>) -> Int` function. You don’t have to write your properties down as executable KotlinTest code—an informal description is fine. Here are some ideas to get you started:

+   - Reversing and summing a list should give the same result as summing the original, non-reversed list.
+   - What should the sum be if all elements of the list are the same value?
+   - Do any other properties spring to mind?

# EXERCISE 8.2

What properties specify a function that finds the maximum of a `List<Int>`?

Property-based testing libraries often come equipped with other useful features. We’ll talk more about some of these features later, but just to give an idea of what’s possible:

+   - *Test case minimization* — In the event of a failing test, the framework tries increasingly smaller dataset sizes until it finds the *smallest* dataset that still fails, this being more illuminating for diagnosing failures. For instance, if a property fails for a list of size 10, the framework tries smaller lists and reports the smallest list that fails the test.
+   - *Exhaustive test case generation* — We call the set of values that could be produced by some `Gen<A>` the *domain*.³⁸ When the domain is small enough (for instance, if it’s all even integers less than 100), we may exhaustively test all its values rather than generate sample values. If the property holds for all values in a domain, we have an actual *proof* rather than just the absence of evidence to the contrary.

KotlinTest is just one framework that provides property-based testing capabilities. And while there’s nothing wrong with it, we’ll derive our own library in this chapter, starting from scratch. Like in chapter 7, this is mostly for pedagogical purposes, but also partly because we should consider no library to be the final word on any subject. There is certainly nothing wrong with using an existing library like KotlinTest, and existing libraries can be a good source of ideas. But even if you decide you like the existing library’s solution, spending an hour or two playing with designs and writing down some type signatures is a great way to learn more about the domain and understand the design trade-offs.