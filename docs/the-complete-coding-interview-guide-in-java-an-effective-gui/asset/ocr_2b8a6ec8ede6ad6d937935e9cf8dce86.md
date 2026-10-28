![](bbox=[0.05, 0.05, 0.9, 0.25])

# Figure 10.13 – Splitting arrays into two equal parts

We can conclude from this diagram that the first condition that the median value should respect is qLeft + pLeft = qRight + pRight. In other words, qPointer + pPointer = (q.length - qPointer) + (p.length - pPointer).

However, since our arrays aren't the same length ("they can be equal, but this is just a special case that should be covered by our solution as well"), we cannot simply halve both of them. What we can do is assume that p >= q (if they are not given like this, then we just swap them to force this assumption). Furthermore, under the umbrella of this assumption, we can write the following:

```
qPointer + pPointer = (q.length - qPointer) + (p.length - pPointer) →
2 * pPointer = q.length + p.length - 2 * qPointer →
 pPointer = (q.length + p.length)/2 - qPointer
```

So far, pPointer can fall in the middle and we can avoid this by adding 1, which means we have the following starting pointers:

+   • qPointer = ((q.length - 1) + 0)/2
+   • pPointer = (q.length + p.length + 1)/2 - qPointer

If p>=q, then the minimum (q.length + p.length + 1)/2 - qPointer will always lead to pPointer as a positive integer. This will eliminate array-out-of-bounds exceptions and respect the first condition as well.