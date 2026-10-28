Creating the buckets can be done in several ways. One approach relies on defining a number of buckets and filling each bucket with a specific range of elements from the given array (this is known as *scattering*). Next, each bucket is sorted (via bucket sorting or other sorting algorithms). Finally, the elements are collected from each bucket to obtain the sorted array (this is known as *gathering*). This is also known as the *scatter-sort-gather* technique and is exemplified in the following diagram. Here, we are using bucket sort on the 4, 2, 11, 7, 18, 3, 14, 7, 4, 16 array:

![](bbox=[0.21, 0.25, 0.78, 0.58])

Figure 14.5 – Bucket Sort via the scatter-sort-gather approach

So, as the preceding diagram reveals, we have defined four buckets for the elements in intervals; that is, 0-5, 5-10, 10-15, and 15-20. Each element of the given array fits into a bucket. After distributing all the elements of the given array into buckets, we sort each bucket. The first bucket contains elements 2, 3, 4, and 4. The second bucket contains elements 7, 7, and so on. Finally, we gather the elements from the buckets (from left to right) and we obtain the sorted array; that is, 2, 3, 4, 4, 7, 7, 11, 14, 16, 18.

So, for this, we can write the following pseudocode:

```
sort(array)
    create N buckets each of which can hold a range of elements
    for all the buckets
        initialize each bucket with 0 values
```