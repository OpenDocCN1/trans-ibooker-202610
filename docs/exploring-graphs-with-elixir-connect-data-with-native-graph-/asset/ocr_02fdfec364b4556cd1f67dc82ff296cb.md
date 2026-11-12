# Iteration on Steroids

We write iterations all the time to process a list of objects and to work with sets and maps. The iterators we're used to in Java are familiar and primitive, but not simple. Not only do they take a few lines of code to work with but they're also hard to compose.

How do we iterate and print each element in a collection? We could use a for loop. How do we select some elements from a collection? With the same for loop, but some extra mutable variables have to step in to support the operation. Now after selecting the values, how do we reduce the results to a single value, such as a minimum, a maximum, or an average? More looping, more mutable variables.

That's like having a jack-of-all-iterations, but a master of none. Java now provides specialized internal iterators for various operations: one to simply loop, one to map data values, one to filter out select values, one to reduce, and several convenience functions to pick the min, max, average, and so on. In addition, these operations are easy to compose so we can combine a variety of them to implement the business logic with greater ease and less code. When we're done, the code is easier to understand as it logically flows through the sequence described in the problem. We'll see several examples of this in Chapter 2, Using Collections, and throughout the book.

# Enforcing Policies

Policies rule enterprise applications. For instance, we may have to ensure an operation has proper security credentials. We may have to ensure that transactions run fast and