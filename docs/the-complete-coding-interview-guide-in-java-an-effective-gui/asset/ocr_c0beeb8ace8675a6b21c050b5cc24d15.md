However, as you can see, there is a method called peekAt() that returns the element at the given index in O(1). If the given index is beyond the bounds of this list, then peekAt() returns -1. Write a snippet of code that returns the index at which an element, p, occurs.

**Solution:** When we must search in a sorted data structure (for example, in a sorted array), we know that Binary Search is the proper choice. So, can we use Binary Search in this case? Since the given list is sorted and we can access any element of it in O(1) time, this means that Binary Search should be a good choice. Apart from that, the Binary Search algorithm requires the size of the data structure since we have to halve the search space (for example, list.size()/2) to find the middle point. The given data structure (list) doesn’t reveal its size.

So, the problem is reduced to finding the size of this list. We know that peekAt() returns -1 if the given index is beyond the bounds of this list, so we can loop the list and count the iterations until peekAt() returns -1. When peekAt() returns -1, we should know the size of the list, so we can apply the Binary Search algorithm. Instead of looping the list element by element (linear algorithm), we can try to do so exponentially. So, instead of looping peekAt(1), peekAt(2), peekAt(3), peekAt(4) ..., we loop peekAt(1), peekAt(2), peekAt(4), peekAt(8), .... In other words, instead of doing this in O(n) time, we can do so in O(log n) time, where n is the size of the list. We can do this because the given list is sorted!

The following code should clarify this approach and the remaining details:

```
public static int search(SizelessList sl, int element) {
    int index = 1;
    while (sl.peekAt(index) != -1
        && sl.peekAt(index) < element) {
        index *= 2;
    }
    return binarySearch(sl, element, index / 2, index);
}

private static int binarySearch(SizelessList sl,
    int element, int left, int right) {
    int mid;
```