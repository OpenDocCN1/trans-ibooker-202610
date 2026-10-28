An implementation of this pseudocode may look as follows:

```
/* Scatter-Gather approach */
public static void sort(int[] arr) {

    // get the maximum value of the given array
    int max = arr[0];
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
    }

    // create max buckets
    int[] bucket = new int[max + 1];

    // the bucket[] is automatically initialized with 0s,
    // therefore this step is redundant
    for (int i = 0; i < bucket.length; i++) {
        bucket[i] = 0;
    }

    // scatter elements in buckets
    for (int i = 0; i < arr.length; i++) {
        bucket[arr[i]]++;
    }

    // gather elements from the buckets
    int p = 0;
    for (int i = 0; i < bucket.length; i++) {
        for (int j = 0; j < bucket[i]; j++) {
            arr[p++] = i;
        }
    }
}
```