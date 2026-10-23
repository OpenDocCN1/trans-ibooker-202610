in total. Crucially, we just figured this out without processing the individual units of this paint stroke. Before we continue, don’t forget to update our stored rightmost position to 16.

Our third paint stroke is like the second in that it starts prior to our stored rightmost position. Unlike the second paint stroke, however, its ending position does not extend past our stored rightmost position at all. So, this paint stroke adds no new blue units, and our stored rightmost position is still 16. Again, we figured this out without grinding through each of this paint stroke’s positions!

Be careful with the fourth paint stroke. It does *not* add 19 − 16 = 3 new blue units. We have to *treat* this paint stroke differently because its starting position is to the right of our stored rightmost position. In this case, we don’t use the stored rightmost position at all, calculating instead 19 − 18 = 1 new blue unit, and 13 blue units in total. We also update our stored rightmost position to 19.

The only question is how we sort the paint strokes in our Python code. We need to sort them by their starting position; if multiple paint strokes have the same starting position, then we want to sort those by their ending position.

That is, we want to take a list like this:

```
[[18, 19], [4, 16], [4, 14], [5, 12]]
```

and produce this:

```
[[4, 14], [4, 16], [5, 12], [18, 19]]
```

Happily, as we discovered in “Task 4: Sort Boxes” in Chapter 6, the list sort method works in exactly this way. When given a list of lists, sort sorts using the first values in each list; when those values are tied, the lists are further sorted using the second values. Check it out:

```
>>> strokes = [[18, 19], [4, 16], [4, 14], [5, 12]]
>>> strokes.sort()
>>> strokes
[[4, 14], [4, 16], [5, 12], [18, 19]]
```

Algorithm: check. Sorting: check. We’re in great shape! Just one more thing we’d like to know before we see the code: what will be its big O efficiency? We need to read the *q* queries; that takes *O(q)* time. Then we need to sort the queries; that takes *O(q log q)* time. Finally, we