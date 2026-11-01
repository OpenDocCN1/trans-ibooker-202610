29. Using `unfold` to define `constant` and `ones` means that we don’t get sharing as in the recursive definition `fun ones(): Stream<Int> = Stream.cons({ 1 }, { ones() })`. The recursive definition consumes constant memory even if we keep a reference to it around while traversing it, while the `unfold`-based implementation does not. Preserving sharing isn’t something we usually rely on when programming with streams, since it’s extremely delicate and not tracked by the types. For instance, sharing is destroyed when calling even `xs.map { x -> x }`.

30. Actually, pseudo-random, but we’ll ignore this distinction for our purposes.

31. Kotlin API link: bit.ly/35MLFhz

32. Recall that `Pair<A, B>` is the type of two-element tuples, and given `p: Pair<A, B>`, you can use `p.first` to extract the `A` and `p.second` to extract the `B`.

33. An efficiency loss comes with computing next states using pure functions, because it means we can’t actually mutate the data in place. Here, it’s not really a problem since the state is just a single `Long` that must be copied. This loss of efficiency can be mitigated by using efficient purely functional data structures. It’s also possible in some cases to mutate the data in place without breaking referential transparency, which we’ll talk about in part 4 of this book.

34. We’ll use the term *logical thread* somewhat informally to mean a computation that runs concurrently with the main execution thread of our program. There need not be a one-to-one correspondence between logical threads and OS threads. We may have a large number of logical threads mapped onto a smaller number of OS threads via thread pooling, for instance.

35. We do mean algebra in the mathematical sense of one or more sets, together with a collection of functions operating on objects of these sets, and a set of *axioms*. Axioms are statements assumed true from which we can derive other *theorems* that must also be true. In our case, the sets are particular types like `Par<A>` and `List<Par<A>>`, and the functions are operations like `map2`, `unit`, and `sequence`.

36. docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/Executor.html

37. This last way of defining new laws is probably the weakest, since it can be too easy to just have the laws reflect the implementation, even if the implementation is buggy or requires all sorts of unusual side conditions that make composition difficult.

38. This is the same usage of “domain” as the domain of a function (en.wikipedia.org/wiki/Domain_of_a_function)—generators describe possible inputs to functions we’d like to test. Note that we’ll also still sometimes use “domain” in the more colloquial sense, to refer to a subject or area of interest, for example, “the domain of functional parallelism” or “the error-handling domain.”

39. www.scalacheck.org

40. hackage.haskell.org/package/QuickCheck