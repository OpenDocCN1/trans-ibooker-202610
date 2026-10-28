```
if (index != 0 && intervals[index - 1].start
    <= intervals[i].end) {
    while (index != 0 && intervals[index - 1].start
        <= intervals[i].end) {

        // merge the previous interval with
        // the current interval
        intervals[index - 1].end = Math.max(
            intervals[index - 1].end, intervals[i].end);
        intervals[index - 1].start = Math.min(
            intervals[index - 1].start, intervals[i].start);
        index--;
    }
    // Step 2b
} else {
    intervals[index] = intervals[i];
}

index++;
}

// print the result
for (int i = 0; i < index; i++) {
    System.out.print(intervals[i] + " ");
}
}
```

The runtime of this code is O(n log n) with an auxiliary space of O(1). The complete application is called *MergeIntervals*.