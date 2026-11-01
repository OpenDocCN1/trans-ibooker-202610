![](bbox=[0.13, 0.06, 0.49, 0.23])

Figure 10.3 A balanced fold splits the workload into equal groups for efficient processing

Note that the balanced fold allows for parallelism because the two inner combine calls are independent and can be run simultaneously. But beyond that, the more balanced tree structure can be more efficient in cases where the cost of each combine is proportional to the size of its arguments. For instance, consider the runtime performance of this expression:

```
listOf("lorem", "ipsum", "dolor", "sit")
.foldLeft("") { a, b -> a + b }
```

At every step of the fold, we’re allocating the full intermediate String only to discard it and allocate a larger string in the next step. Recall that String values are immutable, and that evaluating a + b for strings a and b requires allocating a fresh character array and copying both a and b into this new array. It takes time proportional to a.length + b.length.

We can confirm this by tracing through the evaluation of the preceding expression:

```
listOf("lorem", "ipsum", "dolor", "sit")
    .foldLeft("") { a, b -> a + b }

listOf("ipsum", "dolor", "sit")
    .foldLeft("lorem") { a, b -> a + b }

listOf("dolor", "sit")
    .foldLeft("loremipsum") { a, b -> a + b }

listOf("sit")
    .foldLeft("loremipsumdolor") { a, b -> a + b }

listOf<String>()
    .foldLeft("loremipsumdolorsit") { a, b -> a + b }

"loremipsumdolorsit"
```

Note the intermediate strings being created in each step and then immediately being discarded. A more efficient strategy would be the balanced fold as described earlier. Here we combine the sequence in halves, first constructing "loremipsum" and "dolorsit", then finally adding those together to form "loremipsumdolorsit".