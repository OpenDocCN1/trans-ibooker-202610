+   3. For the rest of intervals, do the following:
+      a. If the current interval does not overlap with the interval from the top of the stack, then push it into the stack.
+      b. If the current interval overlaps with the interval from the top of the stack and the end of the current interval is greater than that of the stack top, then update the top of the stack with the end of the current interval.
+   4. At the end, the stack contains the merged intervals.

In terms of code, this algorithm looks as follows:

```
```java
public static void mergeIntervals(Interval[] intervals) {

    // Step 1
    java.util.Arrays.sort(intervals,
            new Comparator<Interval>() {
        public int compare(Interval i1, Interval i2) {
            return i1.start - i2.start;
        }
    });

    Stack<Interval> stackOfIntervals = new Stack();

    for (Interval interval : intervals) {

        // Step 3a
        if (stackOfIntervals.empty() || interval.start
                > stackOfIntervals.peek().end) {
            stackOfIntervals.push(interval);
        }

        // Step 3b
        if (stackOfIntervals.peek().end < interval.end) {
            stackOfIntervals.peek().end = interval.end;
        }
    }
}
```
```