Heap Sort is not a stable algorithm. A stable algorithm guarantees the order of duplicate elements. The complete application is called *HeapSort*. This application contains an implementation based on `Comparator` as well – this is useful for sorting objects.

# Merge Sort

Now, let’s discuss the Merge Sort algorithm. The time complexity cases are as follows: best case O(n log n), average case O(n log n), worst case O(n log n). The space complexity may vary, depending on the chosen data structures (it can be O(n)).

The Merge Sort algorithm is a recursive algorithm based on the famous *divide and conquer* strategy. Considering that you’ve been given an unsorted array, applying the Merge Sort algorithm requires you to continually split the array in half until we obtain empty sub-arrays or sub-arrays that contains a single element (this is *divide and conquer*). If a sub-array is empty or contains one element, it is sorted by its definition – this is the recursion *base case*.

If we haven’t reached the *base case* yet, we divide both these sub-arrays again and attempt to sort them. So, if the array contains more than one element, we split it and we recursively invoke the sort operation on both sub-arrays. The following diagram shows the splitting process for the 52, 28, 91, 19, 76, 33, 43, 57, 20 array:

![](bbox=[0.22, 0.565, 0.785, 0.785])

Figure 14.2 – Splitting the given array in the Merge Sort algorithm

Once the splitting is done, we call the fundamental operation of this algorithm: the *merge* operation (also known as the *combine* operation). Merging is the operation of taking two smaller sorted sub-arrays and combining them into a single, sorted, new sub-array. This is done until the entire given array is sorted. The following diagram shows the merging operation for our array: