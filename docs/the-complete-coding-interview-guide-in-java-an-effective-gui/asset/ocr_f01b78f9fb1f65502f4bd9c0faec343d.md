Consider the array 4, 2, 5, 1, 6, 7, 3. Here, we're going set the pivot as the end element. The following diagram depicts how Quick Sort works:

![](bbox=[0.18, 0.22, 0.82, 0.55])

Figure 14.4 – Quick Sort

Step 1: We choose the last element as the pivot, so 3 is the pivot. Partitioning begins by locating two position markers – let’s call them i and m. Initially, both point to the first element of the given array. Next, we compare the element at position i with the pivot, so we compare 4 with 3. Since 4 > 3, there is nothing to do, and i becomes 1 (i++), while m remains 0.

Step 2: We compare the element at position i with the pivot, so we compare 2 with 3. Since 2<3, we swap the element at position m with the element at position i, so we swap 4 with 2. Both m and i are increased by 1, so m becomes 1 and i becomes 2.

Step 3: We compare the element at position i with the pivot, so we compare 5 with 3. Since 5 > 3, there is nothing to do, so i becomes 3 (i++), while m remains as 1.

Step 4: We compare the element on position i with the pivot, so we compare 1 with 3. Since 1 < 3, we swap the element at position m with the element at position i, so we swap 1 with 4. Both m and i are increased by 1, so m becomes 2 and i becomes 4.

Step 5 and 6: We continue to compare the element at position i with the pivot. Since 6>3 and 7 > 3, there is nothing to do at these two steps. After these steps, i=7.