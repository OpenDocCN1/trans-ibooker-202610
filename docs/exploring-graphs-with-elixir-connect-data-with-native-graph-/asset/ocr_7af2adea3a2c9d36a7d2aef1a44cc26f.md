The arguments aren't evaluated before we enter the lazyEvaluator method. The second call to evaluate was skipped in this version. This example call of lazyEvaluator takes only about two seconds, whereas the previous call to eagerEvaluator took about four seconds.

We saw the cost savings of the lazy evaluation. This technique is helpful when we have to evaluate a large number of methods or if method evaluations are time/resource consuming.

This technique can significantly boost performance, but its disadvantage is it burdens the caller with packaging the calls in a lambda expression. Sure, lambda expressions are concise, but they're a hindrance compared to the usual way of passing arguments. In some contexts, we may be able to use method references instead of lambda expressions, and this can make the code a bit more concise and ease the burden a little.

The lazy solutions so far have helped us make code more efficient. Next, we'll use it purely for convenience.