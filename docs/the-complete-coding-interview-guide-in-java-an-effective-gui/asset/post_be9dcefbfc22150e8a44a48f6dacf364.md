
The complete application is called *MergeTwoSortedArrays*. If you’d like to check/remember how to merge *k* sorted arrays, then revisit *Chapter 10, Arrays and Strings, the Merging k sorted arrays in O(nk log k) coding challenge.*

## Coding challenge 2 – Grouping anagrams together

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

- `void sort(Object[] a)`
- `<T> void sort(T[] a, Comparator<? super T> c)`

If we convert a string (word) into char [], then we can sort its characters and return the new string via the following helper method:

```
// helper method for sorting the chars of a word
private static String sortWordChars(String word) {

    char[] wordToChar = word.toCharArray();
    Arrays.sort(wordToChar);

    return String.valueOf(wordToChar);
}
```

Next, we just need a Comparator that indicates that two strings that are anagrams of each other are equivalent:

```
public class Anagrams implements Comparator<String> {

    @Override
    public int compare(String s1, String s2) {
        return sortStringChars(s1).compareTo(sortStringChars(s2));
    }
}
```

Finally, we sort the given array of strings (words) via this compareTo() method:

```
Arrays.sort(words, new Anagrams());
```

However, the problem doesn’t actually ask us to sort the given array of anagrams; the problem asks us to print the anagrams grouped together. For this, we can rely on hashing (if you are not familiar with the concept of hashing, then please read Chapter 6, Object-Oriented Programming, the Hash table problem). In Java, we can use hashing via the built-in HashMap implementation, so there is no need to write a hashing implementation from scratch. But how is this HashMap useful? What should we store in an entry (key-value pair) of this map?

Each group of anagrams converges to the same sorted version (for example, the group of anagrams containing the strings (words) "slat", "salt" and "last" have the unique and common sorted version as "alst"). Being unique, the sorted version is a good candidate to be the key in our map. Next, the value represents the list of anagrams. So, the algorithm is quite simple; it contains the following steps:

1. Loop over the given array of words.
2. Sort the characters of each word.
3. Populate the map (add or update the map).
4. Print the result.

In code lines:

```
/* Group anagrams via hashing (O(nm log m) */
public void printAnagrams(String words[]) {

    Map<String, List<String>> result = new HashMap<>();

    for (int i = 0; i < words.length; i++) {

        // sort the chars of each string
        String word = words[i];
        String sortedWord = sortWordChars(word);

        if (result.containsKey(sortedWord)) {
            result.get(sortedWord).add(word);
        } else {
            // start a new group of anagrams
            List<String> anagrams = new ArrayList<>();
            anagrams.add(word);
            result.put(sortedWord, anagrams);
        }
    }

    // print the result
    System.out.println(result.values());
}
```

If n is the number of strings (words) and each string (word) has a maximum of m characters, then the time complexity of the preceding two approaches is O(nm log m).

Can we do this better? Well, to do this better, we have to identify the issue of the preceding two approaches. The issue consists of the fact that we sort every string (word) and that this will cost us extra time. However, we can use an additional char [] to count up the number of occurrences (frequency) of each character in a string (word). After we build this char [], we convert it into a String to obtain the key that we have to search for in HashMap. Since Java handles char types the same as it does (unsigned) short, we can make calculations with char. Let's see the code (the wordToChar array tracks the frequency of characters from a to z for each string (word) in the given array):

```
/* Group anagrams via hashing (O(nm)) */
public void printAnagramsOptimized(String[] words) {

    Map<String, List<String>> result = new HashMap<>();

    for (int i = 0; i < words.length; i++) {

        String word = words[i];
        char[] wordToChar = new char[RANGE_a_z];

        // count up the number of occurrences (frequency)
        // of each letter in 'word'
        for (int j = 0; j < word.length(); j++) {
            wordToChar[word.charAt(j) - 'a']++;
        }

        String computedWord = String.valueOf(wordToChar);

        if (result.containsKey(computedWord)) {
            result.get(computedWord).add(word);
        } else {
            List<String> anagrams = new ArrayList<>();
            anagrams.add(word);
            result.put(computedWord, anagrams);
        }
    }
}
```

```
System.out.println(result.values());
}
```

If *n* is the number of strings (words) and each string (word) contains a maximum of *m* characters, then the time complexity of the preceding two approaches is O(nm). If you need to support more characters, not just from *a* to *z*, then use an `int []` array and `codePointAt()` – more details are available in *Chapter 10, Arrays and Strings*, in the *Extracting code points of surrogate pairs* coding challenge. The complete application is called *GroupSortAnagrams.*

## Coding challenge 3 – List of unknown size

**Problem**: Consider you’ve been given a data structure representing a sorted list of unknown size (this means that there is no `size()` or similar method) containing only positive numbers. The code for this list is as follows:

```
public class SizelessList {

    private final int[] arr;

    public SizelessList(int[] arr) {
        this.arr = arr.clone();
    }

    public int peekAt(int index) {
        if (index >= arr.length) {
            return -1;
        }

        return arr[index];
    }
}
```

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
