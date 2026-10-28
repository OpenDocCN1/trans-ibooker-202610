
```
}

int[] out = new int[arr.length];
for (int i = arr.length - 1; i >= 0; i--) {
    bucket = (int) (((arr[i] - min) / exp) % radix);
    out[--buckets[bucket]] = arr[i];
}

System.arraycopy(out, 0, arr, 0, arr.length);
}
```

The time complexity of Radix Sort depends on the algorithm that’s used to sort the digits (remember that this can be any stable sorting algorithm). Since we are using the Counting Sort algorithm, the time complexity is O(d(n+b)), where n is the number of elements, d is the number of digits, and b is the radix or base (in our case, the base is 10). The space complexity is O(n+b).

The complete application is called RadixSort. Well, so far, we’ve managed to cover the top five sorting algorithms that occur in technical interviews. Now, let’s quickly provide an overview of the searching algorithms.

## Searching algorithms

The main searching algorithm that occurs in interviews as a standalone problem or part of another problem is the Binary Search algorithm. The best case time complexity is O(1), while the average and worst case is O(log n). The worst case auxiliary space complexity of Binary Search is O(1) for the iterative implementation and O(log n) for the recursive implementation due to the call stack.

The Binary Search algorithm relies on the divide and conquer strategy. Mainly, this algorithm debuts by dividing the given array into two sub-arrays. Furthermore, it discards one of these sub-arrays and operates on the other one iteratively or recursively. In other words, at each step, this algorithm halves the search space (which is initially the whole given array).

So, these algorithms describe the steps for looking for element x in an array, a. Consider a sorted array, a, that contains 16 elements, as shown in the following image:

![](img/6709a71136a6b327c831c6c15aff2f6a_602_0.png)

Figure 14.8 – Ordered array containing 16 elements

First, we compare x with the midpoint of the array, p. If they are equal, we return. If x > p, then we search the right-hand side of the array and discard the left-hand side (the search space is the right-hand side of the array). If x < p, then we search on the left-hand side of the array and discard the right-hand side (the search space is the left-hand side of the array). The following is a graphical representation of the Binary Search algorithm for finding the number 17:

![](img/6709a71136a6b327c831c6c15aff2f6a_602_1.png)

Figure 14.9 – The Binary Search algorithm

Notice that we start with 16 elements and end with 1. After the first step, we are down to 16/2 = 8 elements. At the second step, we are down to 8/2 = 4 elements. At the third step, we are down to 4/2 = 2 elements. And, at the final step, we find the searched number, 17. If we put this algorithm into pseudocode, then we will obtain something similar to the following:

```
search 17 in {1, 4, 5, 7, 10, 16, 17, 18, 20,
            23, 24, 25, 26, 30, 31, 33}
compare 17 to 18 -> 17 < 18
```

```
search 17 in {1, 4, 5, 7, 10, 16, 17, 18}
    compare 17 to 7 -> 17 > 7
    search 17 in {7, 10, 16, 17}
        compare 17 to 16 -> 17 > 16
        search 17 in {16, 17}
            compare 17 to 17
            return
```

The iterative implementation is listed here:

```
public static int runIterative(int[] arr, int p) {

    // the search space is the whole array
    int left = 0;
    int right = arr.length - 1;

    // while the search space has at least one element
    while (left <= right) {

        // half the search space
        int mid = (left + right) / 2;

        // if domain overflow can happen then use:
        // int mid = left + (right - left) / 2;
        // int mid = right - (right - left) / 2;

        // we found the searched element
        if (p == arr[mid]) {
            return mid;
        } // discard all elements in the right of the
            // search space including 'mid'
        else if (p < arr[mid]) {
            right = mid - 1;
        } // discard all elements in the left of the
            // search space including 'mid'
        else {
            left = mid + 1;
```

```
}
}

// by convention, -1 means element not found into the array
return -1;
}
```

The complete application is called *BinarySearch*. It also contains the recursive implementation of the Binary Search algorithm. In *Chapter 10, Arrays and Strings*, you can find different coding challenges that take advantage of the Binary Search algorithm.

## Coding challenges

So far, we’ve covered the most popular sorting and searching algorithms that are encountered in technical interviews. It is advised that you practice these algorithms since they may occur as standalone problems that require the pseudocode or the implementation.

That being said, let’s tackle 18 problems that are related to sorting and searching algorithms.

### Coding challenge 1 – Merging two sorted arrays

**Amazon, Google, Adobe, Microsoft, Flipkart**

**Problem:** Consider you’ve been given two sorted arrays, *p* and *q*. The *p* array is large enough to fit *q* at the end of it. Write a snippet of code that merges *p* and *q* in a sorted order.

**Solution:** It is important to highlight the fact that *p* has enough space at the end to fit *q*. This suggests that the solution shouldn’t involve any auxiliary space. The solution should output the result of merging *p* and *q* in a sorted order by inserting the elements from *q* in *p* in order.

Mainly, we should compare the elements from *p* and *q* and insert them into *p* in order until we’ve processed all the elements in *p* and *q*. Let’s take a look at a meaningful diagram that reveals this action (*p* contains elements -1, 3, 8, 0, 0, while *q* contains elements 2, 4):

# Figure 14.10 – Merging two sorted arrays

Let’s see this test case step by step (let’s denote the index of the last element from p with pIdx and the index of the last element from q with qIdx). In the previous diagram, pIdx=2 (corresponding to element 8) and qIdx=1 (corresponding to element 4).

Step 1: We compare the last element from p (the element at index pIdx) with the last element from q (the element at index qIdx), so we compare 8 with 4. Since 8 > 4, we copy 8 to the end of p. Since both arrays are sorted, 8 is the maximum of these arrays, so it must go to the last position (index) in p. It will occupy an empty slot in p (remember that p is large enough to fit q at its end). We decrease pIdx by 1.

Step 2: We compare the last element from p (the element at index pIdx) with the last element from q (the element at index qIdx), so we compare 3 with 4. Since 3 < 4, we copy 4 to the end of p. We decrease qIdx by 1.

Step 3: We compare the last element from p (the element at index pIdx) with the last element from q (the element at index qIdx), so we compare 3 with 2. Since 3 > 2, we copy 3 to the end of p. We decrease pIdx by 1.

Step 4: We compare the last element from p (the element at index pIdx) with the last element from q (the element at index qIdx), so we compare -1 with 2. Since -1 < 2, we copy 2 to the end of p. We decrease qIdx by 1. There are no more elements to compare and p is sorted.

Check this out! After each comparison, we insert the elements at the end of p. This way, we don’t need to shift any elements. However, if we choose to insert the elements at the beginning of p, then we must shift the elements backward to make room for each inserted element. This is not efficient!

Now, it’s time to see the implementation of this algorithm:

```
public static void merge(int[] p, int[] q) {

    int pLast = p.length - q.length;
    int qLast = q.length;

    if (pLast < 0) {
        throw new IllegalArgumentException("p cannot fit q");
    }

    int pIdx = pLast - 1;
    int qIdx = qLast - 1;
    int mIdx = pLast + qLast - 1;

    // merge p and q
    // start from the last element in p and q
    while (qIdx >= 0) {

        if (pIdx >= 0 && p[pIdx] > q[qIdx]) {
            p[mIdx] = p[pIdx];
            pIdx--;
        } else {
            p[mIdx] = q[qIdx];
            qIdx--;
        }

        mIdx--;
    }
}
```
