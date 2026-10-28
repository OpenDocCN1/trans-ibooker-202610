```
arr[p++] = j;
}
}
}

```

Another approach to creating buckets consists of putting a single element into a bucket, as shown in the following diagram (this time, there is no sorting involved):

![](bbox=[0.05, 0.18, 0.95, 0.55])

# Figure 14.6 – Bucket Sort via the scatter-gather approach

In this scatter-gather approach, we store the number of occurrences of an element in each bucket, not the element itself, while the position (the index) of the bucket represents the element value. For example, in bucket number 2, we store the number of occurrences of element 2, which in array 4, 2, 8, 7, 8, 2, 2, 7, 4, 9 occurs three times. Since elements 1, 3, 5, and 6 are not present in the given array, their buckets are empty (have 0s in them). The gathering operation collects the elements from left to right and obtains the sorted array.

```
sort (array)
create N buckets each of which can track a counter of a single element
for all the buckets
initialize each bucket with 0 values
for all the buckets
put elements into buckets matching a single element per bucket
for all the buckets
gather elements from each bucket
end
```