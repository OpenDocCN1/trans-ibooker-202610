# Example 17 – string comparison

What is Big O for the following snippet of code? (note that a is an array, and be sure to carefully read the comments):

```
String[] sortArrayOfString(String[] a) {
    for (int i = 0; i < a.length; i++) {
        // sort each string via O(n log n) algorithm
    }

    // sort the array itself via O(n log n) algorithm

    return a;
}
```

sortArrayOfString() receives an array of String and performs two major actions. It sorts each string from this array and the array itself. Both sorts are accomplished via algorithms whose runtime is expressed as O(n log n).

Now, let's focus on the for loop and see the wrong answer that is commonly given by candidates. We already know that sorting a single string gives us O(n log n). Doing this for each string means O(n) * (n log n) = O(n*n log n) = O(n² log n). Next, we sort the array itself, which is also given as O(n log n). Putting all of the results together, the total Big O value is O(n² log n) + O(n log n) = O(n² log n + n log n), which is O(n² log n) since n log n is a non-dominant term. However, is this correct? The short answer is no! But why not?! There are two major mistakes that we've done: we've used n to represent two things (the size of the array and the length of the string) and we assumed that comparing String requires a constant time as is the case for fixed-width integers.

Let's detail the first problem. So, sorting a single string gives us O(n log n), where n represents the length of that string. We sort a.length strings, so n now represents the size of the array. This is where the confusion comes from, because when we say that the for loop is O(n² log n), to which n are we referring to? Since we are working with two variables, we need to denote them differently. For example, we can consider the following:

+   - s: The length of the longest String.
+   - p: The size of the array of String.