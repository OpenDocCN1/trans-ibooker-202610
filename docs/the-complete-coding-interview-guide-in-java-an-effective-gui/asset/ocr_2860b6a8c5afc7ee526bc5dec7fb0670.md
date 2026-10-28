Finally, we have to find the maximum of the left-hand side and the minimum of the right-hand side and, based on the odd or even length of the two arrays, return the maximum of the left-hand side or the average of the maximum of the left-hand side and the minimum of the right-hand side. We know that the maximum of the left-hand side is max(q[qPointer-1], p[pPointer-1]), so max(11, 17) = 17. We also know that the minimum of the right-hand side is min(q[qPointer], p[pPointer]), so min(65, 18) = 18. Since the sum of lengths is 7 + 9 = 16, we compute that the median value is the average of these two, so avg(17, 18) = 17.5. We can visualize this as follows:

![](bbox=[0.13, 0.26, 0.88, 0.35])

Figure 10.16 – Median value (final result)

Putting this algorithm into code results in the following output:

```
public static float median(int[] q, int[] p) {

    int lenQ = q.length;
    int lenP = p.length;

    if (lenQ > lenP) {
        swap(q, p);
    }

    int qPointerMin = 0;
    int qPointerMax = q.length;
    int midLength = (q.length + p.length + 1) / 2;

    int qPointer;
    int pPointer;

    while (qPointerMin <= qPointerMax) {

        qPointer = (qPointerMin + qPointerMax) / 2;
        pPointer = midLength - qPointer;
```