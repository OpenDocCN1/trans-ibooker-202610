```
```java
// print the result
while (!stackOfIntervals.empty()) {
    System.out.print(stackOfIntervals.pop() + " ");
}
}
```
```

The runtime of this code is O(n log n) with an auxiliary space of O(n) for the stack. While the interviewer should be satisfied with this approach, he/she may ask you for optimization. More precisely, can we drop the stack and obtain a complexity space of O(1)?

If we drop the stack, then we must perform the merge operation in-place. The algorithm that can do this is self-explanatory:

+   1. Sort the given intervals in ascending order based on their starts.
+   2. For the rest of the intervals, do the following:
+      a. If the current interval is not the first interval and it overlaps with the previous interval, then merge these two intervals. Do the same for all the previous intervals.
+      b. Otherwise, add the current interval to the output array of intervals.

Notice that, this time, the intervals are sorted in descending order of their starts. This means that we can check whether two intervals are overlapping by comparing the start of the previous interval with the end of the current interval. Let's see the code for this:

```
```java
public static void mergeIntervals(Interval intervals[]) {

    // Step 1
    java.util.Arrays.sort(intervals,
        new Comparator<Interval>() {
            public int compare(Interval i1, Interval i2) {
                return i2.start - i1.start;
            }
        });

    int index = 0;

    for (int i = 0; i < intervals.length; i++) {

        // Step 2a
```
```