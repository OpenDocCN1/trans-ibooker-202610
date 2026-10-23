what we did in the past, we’ll always have at least one available grass type.

Let’s add pasture 5 to our table:

| Pasture | Grass type |
|---|---|
| 1 | 1 |
| 2 | 2 |
| 3 | 1 |
| 4 | 3 |
| 5 | 4 |

There are three pastures to go. But no cow cares about any of them, so we can just use grass type 1 in each case. That gives us this:

| Pasture | Grass type |
|---|---|
| 1 | 1 |
| 2 | 2 |
| 3 | 1 |
| 4 | 3 |
| 5 | 4 |
| 6 | 1 |
| 7 | 1 |
| 8 | 1 |

We can read the grass types from top to bottom to obtain the correct output for this example. The output is as follows:

12134111

# Top-Down Design