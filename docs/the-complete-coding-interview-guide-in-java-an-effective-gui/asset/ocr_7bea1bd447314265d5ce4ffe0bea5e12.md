The new *middle* is (0 + 1) / 2 = 0. Since *m[0]*≠14, we continue with *step 3*. Since *m[0]<m[1]*, we conclude that the right-half is sorted. We notice that *x > m[middle]* (14 > 11) and *x = m[right]* (14 = 14), so we apply *step 3a*. We ignore the left-half and we set *left = middle + 1 = 0 + 1 = 1*. We apply *step 1* again.

The new *middle* is (1 + 1) / 2 = 1. Since *m[1]=14*, we stop and return 1 as the index of the array where we found the searched value.

Let's put this into code:

```
```java
public static int find(int[] m, int x) {

    int left = 0;
    int right = m.length - 1;

    while (left <= right) {

        // halve the search space
        int middle = (left + right) / 2;

        // we found the searched value
        if (m[middle] == x) {
            return middle;
        }

        // check if the right-half is sorted (m[middle ... right])
        if (m[middle] <= m[right]) {

            // check if x is in m[middle ... right]
            if (x > m[middle] && x <= m[right]) {
                left = middle + 1;  // search in the right-half
            } else {
                right = middle - 1;  // search in the left-half
            }
        } else { // the left-half is sorted (A[left ... middle])
            // check if x is in m[left ... middle]
            if (x >= m[left] && x < m[middle]) {
                right = middle - 1; // search in the left-half
```
```