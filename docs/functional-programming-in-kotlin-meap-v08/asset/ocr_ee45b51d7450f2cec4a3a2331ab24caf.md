and rewrapping in the `Id` constructor. With `State`, the most current state gets passed from one statement to the next. With the `Option` monad, a statement may return `None` and terminate the program. With the `List` monad, a statement may return many results, which causes statements that follow it to potentially run multiple times, once for each result.

The `Monad` contract doesn’t specify *what* is happening between the lines, only that whatever *is* happening satisfies the laws of associativity and identity.

# EXERCISE 11.19 (Hard)

To cement your understanding of monads, give a monad instance for the `Reader` data type and explain what it means. Also, take some time to answer the following questions:

+   - what are its primitive operations?
+   - What is the action of `flatMap`?
+   - What meaning does it give to monadic functions like `sequence`, `join`, and `replicateM`?
+   - What meaning does it give to the monadic laws?

In this chapter, we took a pattern that we’ve seen repeated throughout the book and then unified it under a single concept: monad. This allowed us to write a number of combinators once and for all, for many different data types that at first glance don’t seem to have anything in common. We discussed *monad laws* that all monads satisfy from various perspectives, then finally developed some insight into what the broad term means.

An abstract topic like this can’t be fully understood all at once. It requires an iterative approach where you keep revisiting the topic from different perspectives. When you discover new monads or new applications of them, or see them appear in a new context, you’ll inevitably gain new insight. And each time it happens, you might think to yourself, "OK, I thought I understood monads before, but now I *really* get it.". Don’t be fooled!