In the following exercise, we shall look at the implementation of the binary search algorithm.

# Exercise 18: Binary Search Benchmarks

In this exercise, we will write and benchmark a binary search implementation. Follow these steps to complete this exercise:

+   1. Begin by adding the following headers:
+   ```
+   #include <iostream>
+   #include <vector>
+   #include <chrono>
+   #include <random>
+   #include <algorithm>
+   #include <numeric>
+   ```

+   2. Add the linear search code like so:
+   ```
+   bool linear_search(int N, std::vector<int>& S)
+   {
+       for (auto i : S)
+       {
+           if (i == N)
+               return true;    // Element found!
+       }
+       return false;
+   }
+   ```

+   3. Add the binary search code shown here:
+   ```
+   bool binary_search(int N, std::vector<int>& S)
+   {
+       auto first = S.begin();
+       auto last = S.end();
+   
+       while (true)
+       {
+           // Get the middle element of current range
+           auto range_length = std::distance(first, last);
+           auto mid_element_index = first + std::floor(range_length / 2);
+           auto mid_element = *(first + mid_element_index);
+   
+           // Compare the middle element of current range with N
+   ```