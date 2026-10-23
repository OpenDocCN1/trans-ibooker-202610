8 6
5 4
2 4
3 5
4 1
2 1
5 2

The first line of the test case tells us that we have eight pastures. They’re numbered from 1 to 8. The first line also tells us that we have six cows. The problem doesn’t specify a numbering for the cows, so I’ll just number them starting at 0. The two favorite pastures of each cow are in Table 7-1 for easy reference.

# Table 7-1: Farm Seeding Example, Cows

| Cow | Pasture 1 | Pasture 2 |
|---|---|---|
| 0 | 5 | 4 |
| 1 | 2 | 4 |
| 2 | 3 | 5 |
| 3 | 4 | 1 |
| 4 | 2 | 1 |
| 5 | 5 | 2 |

In this problem, we’re being asked to make n decisions. What grass type should we use for pasture 1? What grass type should we use for pasture 2? Pasture 3? Pasture 4? And so on, all the way up to pasture n. One strategy for these kinds of problems is to make one decision at a time, without making a mistake on any of them. If we manage to finish with decision n and haven’t made any mistakes along the way, then our solution must be correct.

Let’s go through the pastures from 1 to 8 and see if we can assign a grass type to each of them. We need to prioritize choosing small-numbered grass types so that we end with the smallest grass types when interpreted as a number.

What grass type should we choose for pasture 1? The only cows that care about pasture 1 are cows 3 and 4, so we focus only on those two. If we had already chosen grass types for some of these cows’ pastures, then