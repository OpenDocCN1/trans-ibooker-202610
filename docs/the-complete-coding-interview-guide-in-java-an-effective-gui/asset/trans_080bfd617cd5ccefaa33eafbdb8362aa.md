

```java
sort(arr, left, m - 1);
sort(arr, m + 1, right);
}

private static int partition(int[] arr, int left, int right) {
    int pivot = arr[right];
    int m = left;
    for (int i = m; i < right; i++) {
        if (arr[i] <= pivot) {
            swap(arr, i, m++);
        }
    }
    swap(arr, right, m);
    return m;
}
```

快速排序可以交换不相邻的元素，因此它是不稳定的。完整的应用名为 QuickSort。该应用还包含一个基于 Comparator 的实现——这对于对象排序非常有用。

## 桶排序

桶排序（Bucket Sort，或 Bin Sort）是面试中会遇到的另一种排序技术。它在计算机科学中很常用，并且当元素在某个范围内均匀分布时非常有用。时间复杂度的情况如下：最佳和平均情况为 O(n+k)，其中 O(k) 是创建桶的时间（对于链表或哈希表，这将是 O(1)），而 O(n) 是将给定数组的元素放入桶中所需要的时间（对于链表或哈希表，这也是 O(1)）。最坏情况为 O(n²)。空间复杂度为 O(n+k)。

它依赖于将给定数组的元素划分为若干组，这些组称为桶。接下来，使用另一种合适的排序算法或通过递归使用桶排序算法，对每个桶单独进行排序。

创建桶可以通过多种方式完成。一种方法依赖于定义桶的数量，并将给定数组中的特定范围的元素填入每个桶（这被称为*散射*）。接下来，对每个桶进行排序（通过桶排序或其他排序算法）。最后，从每个桶中收集元素，得到排序后的数组（这被称为*聚集*）。这也被称为*散射-排序-聚集*技术，并在下图中示例说明。这里，我们对数组 4, 2, 11, 7, 18, 3, 14, 7, 4, 16 使用桶排序：

![](img/6709a71136a6b327c831c6c15aff2f6a_595_0.png)

图 14.5 – 通过散射-排序-聚集方法进行桶排序

因此，如前面的图所示，我们为区间内的元素定义了四个桶；即 0-5、5-10、10-15 和 15-20。给定数组的每个元素都放入一个桶中。将给定数组的所有元素分配到桶中后，我们对每个桶进行排序。第一个桶包含元素 2、3、4 和 4。第二个桶包含元素 7、7 等。最后，我们（从左到右）从桶中收集元素，得到排序后的数组；即 2、3、4、4、7、7、11、14、16、18。

因此，为此我们可以编写以下伪代码：

```
sort(array)
    create N buckets each of which can hold a range of elements
    for all the buckets
        initialize each bucket with 0 values
    for all the buckets
        put elements into buckets matching the range
    for all the buckets
        sort elements in each bucket
        gather elements from each bucket
end
```

使用列表对该伪代码的一种实现可以如下完成（此代码中调用的 hash() 方法可在本书附带的代码中找到）：

```
/* Scatter-Sort-Gather approach */
public static void sort(int[] arr) {

    // get the hash codes
    int[] hashes = hash(arr);

    // create and initialize buckets
    List<Integer>[] buckets = new List[hashes[1]];
    for (int i = 0; i < hashes[1]; i++) {
        buckets[i] = new ArrayList();
    }

    // scatter elements into buckets
    for (int e : arr) {
        buckets[hash(e, hashes)].add(e);
    }

    // sort each bucket
    for (List<Integer> bucket : buckets) {
        Collections.sort(bucket);
    }

    // gather elements from the buckets
    int p = 0;
    for (List<Integer> bucket : buckets) {
        for (int j : bucket) {
            arr[p++] = j;
        }
    }
}
```

另一种创建桶的方法是将单个元素放入一个桶中，如下图所示（这次不涉及排序）：

![](img/6709a71136a6b327c831c6c15aff2f6a_597_0.png)

图 14.6 – 通过散射-聚集方法进行桶排序

在这种散射-聚集方法中，我们在每个桶中存储元素出现的次数，而不是元素本身，而桶的位置（索引）表示元素的值。例如，在 2 号桶中，我们存储元素 2 的出现次数，在数组 4, 2, 8, 7, 8, 2, 2, 7, 4, 9 中它出现了三次。由于元素 1、3、5 和 6 不在给定数组中，它们的桶是空的（其中为 0）。聚集操作从左到右收集元素，得到排序后的数组。

```
sort (array)
    create N buckets each of which can track a counter of a single element
    for all the buckets
        initialize each bucket with 0 values
    for all the buckets
        put elements into buckets matching a single element per bucket
    for all the buckets
        gather elements from each bucket
end
```

该伪代码的一种实现可能如下所示：

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

桶排序不是一个稳定的算法。稳定算法保证重复元素的顺序。完整的应用名为 *BucketSort*。

## 基数排序

基数排序是一种对整数非常有效的排序算法。在基数排序中，我们通过按数字在数值中的位置对各个位进行分组来对元素排序。接下来，我们通过对每个有效位置上的位进行排序来对元素排序。通常，这是通过计数排序完成的（计数排序算法在 Packt 出版的《*Java 编码问题*》（www.packtpub.com/programming/java-coding-problems）一书中有详细介绍，但你可以在名为 *SortArraysIn14Ways* 的应用中找到它的实现）。主要来说，对位进行排序可以通过任何稳定的排序算法完成。

理解基数排序算法的一个简单方法是借助示例。让我们考虑数组 323, 2, 3, 123, 45, 6, 788。下图揭示了通过依次对个位、十位和百位进行排序来对该数组进行排序的步骤：

![](img/6709a71136a6b327c831c6c15aff2f6a_599_0.png)

图 14.7 – 基数排序

因此，首先，我们根据个位对应的数字对元素排序。其次，我们根据十位对应的数字对元素排序。第三，我们根据百位对应的数字对元素排序。当然，根据数组中的最大数，该过程会继续到千位、万位等，直到没有剩余的位为止。

以下代码是基数排序算法的一种实现：

```java
public static void sort(int[] arr, int radix) {

    int min = arr[0];
    int max = arr[0];
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] < min) {
            min = arr[i];
        } else if (arr[i] > max) {
            max = arr[i];
        }
    }

    int exp = 1;
    while ((max - min) / exp >= 1) {
        countSortByDigit(arr, radix, exp, min);
        exp *= radix;
    }
}

private static void countSortByDigit(
    int[] arr, int radix, int exp, int min) {

    int[] buckets = new int[radix];
    for (int i = 0; i < radix; i++) {
        buckets[i] = 0;
    }

    int bucket;
    for (int i = 0; i < arr.length; i++) {
        bucket = (int) (((arr[i] - min) / exp) % radix);
        buckets[bucket]++;
    }

    for (int i = 1; i < radix; i++) {
        buckets[i] += buckets[i - 1];
```

