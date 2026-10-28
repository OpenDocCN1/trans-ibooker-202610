The complete application is called *MergeTwoSortedArrays*. If you’d like to check/remember how to merge *k* sorted arrays, then revisit *Chapter 10, Arrays and Strings, the Merging k sorted arrays in O(nk log k) coding challenge.*

# Coding challenge 2 – Grouping anagrams together

Adobe, Flipkart

**Problem:** Consider you’ve been given an array of words (containing characters from ‘a’ to ‘z’) representing several mixed anagrams (for example, "calipers", "caret", "slat", "cater", "thickset", "spiracle", "trace", "last", "salt", "bowel", "crate", "loop", "polo", "thickest", "below", "thickets", "pool", "elbow", "replicas"). Write a snippet of code that prints this array so that all the anagrams are grouped together (for example, "calipers", "spiracle", "replicas", "caret", "cater", "trace", "crate", "slat", "last", "salt", "bowel", "below", "elbow", "thickset", "thickest", "thickets", "loop", "polo", "pool").

**Solution:** First, here’s a quick reminder regarding anagrams. Two or more strings (words) are considered to be anagrams if they contain the same characters but in different orders.

Based on the example provided for this problem, let’s define the following array of mixed anagrams:

```
String[] words = {
    "calipers", "caret",  "slat",  "cater",  "thickset",
    "spiracle", "trace",  "last",  "salt",  "bowel",  "crate",
    "loop",  "polo",  "thickest",  "below",  "thickets",
    "pool",  "elbow",  "replicas"
};
```

Since anagrams contain exactly the same characters, this means that if we sort them, then they will be identical (for example, sorting "slat", "salt" and "last" result in "alst"). So, we can say that two strings (words) are anagrams by comparing their sorted versions. In other words, all we need is a sorting algorithm. The most convenient way to do this is to rely on Java’s built-in sorting algorithm, which is Dual-Pivot Quicksort for primitives and TimSort for objects.

The built-in solution is called `sort()` and comes in many different flavors in the `java.util.Arrays` class (15+ flavors). Two of these flavors have the following signatures:

+   - `void sort(Object[] a)`
+   - `<T> void sort(T[] a, Comparator<? super T> c)`