# Coding challenge 12 – First position of first one

Amazon, Google, Adobe

Problem: Consider you’ve been given an array that contains only values of 0 and 1. There is at least a 0 and a 1. All 0s comes first, followed by 1s. Write a snippet of code that returns the index of the first 1 in this array.

Solution: Consider the array arr=[0, 0, 0, 1, 1, 1, 1]. The searched index is 3 since arr[3] is 1, and this is the first 1.

Since 0s comes first, followed by 1s, the array is sorted.

> Note
> Since this is a very common topic in interviews, I’ll say it again: when we have to find something in a sorted array, we have to consider the Binary Search algorithm.

In this case, the Binary Search algorithm can be implemented quite easily. The middle point that’s computed in Binary Search can fall on 0 or 1. Since the array is sorted, if the middle point falls on 0, then we know for sure that the first value of 1 must be on the right-hand side of the middle point, so we discard the left-hand side of the middle point. On the other hand, if the middle point falls on 1, then we know that the first value of 1 must on the left-hand side of the middle point, so we discard the right-hand side of the middle point. The following code clarifies this:

```
public static int firstOneIndex(int[] arr) {
    if (arr == null) {
        return -1;
    }
    int left = 0;
    int right = arr.length - 1;
    while (left <= right) {
        int middle = 1 + (right - left) / 2;
        if (arr[middle] == 0) {
            left = middle + 1;
```