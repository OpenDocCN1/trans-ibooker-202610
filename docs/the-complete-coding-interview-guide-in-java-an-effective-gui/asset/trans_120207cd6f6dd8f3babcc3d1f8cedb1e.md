
完整的应用程序名为 *MergeTwoSortedArrays*。如果你想查看/回顾如何合并 *k* 个有序数组，请重新阅读第10章“数组与字符串”中的“以 O(nk log k) 合并 k 个有序数组”编程挑战。

## 编程挑战2——将变位词分组

Adobe、Flipkart

**问题：** 假设你有一个字符串数组（包含从‘a’到‘z’的字符），表示多个混杂在一起的变位词（例如，"calipers"、"caret"、"slat"、"cater"、"thickset"、"spiracle"、"trace"、"last"、"salt"、"bowel"、"crate"、"loop"、"polo"、"thickest"、"below"、"thickets"、"pool"、"elbow"、"replicas"）。编写一段代码，输出该数组，使所有这些变位词被分组在一起（例如，"calipers"、"spiracle"、"replicas"、"caret"、"cater"、"trace"、"crate"、"slat"、"last"、"salt"、"bowel"、"below"、"elbow"、"thickset"、"thickest"、"thickets"、"loop"、"polo"、"pool"）。

**解法：** 首先，这里快速回顾一下变位词。如果两个或多个字符串（单词）包含相同的字符但顺序不同，则它们被视为变位词。

基于该问题给出的示例，让我们定义以下混杂变位词数组：

```
String[] words = {
    "calipers", "caret",  "slat",  "cater",  "thickset",
    "spiracle", "trace",  "last",  "salt",  "bowel",  "crate",
    "loop",  "polo",  "thickest",  "below",  "thickets",
    "pool",  "elbow",  "replicas"
};
```

由于变位词包含完全相同的字符，这意味着如果我们对它们进行排序，那么它们将会是相同的（例如，对"slat"、"salt"和"last"排序后都会得到"alst"）。因此，我们可以通过比较排序后的版本来判断两个字符串（单词）是否为变位词。换句话说，我们只需要一种排序算法。最方便的方法是依赖Java内置的排序算法，即对于基本类型使用双轴快速排序，对于对象使用TimSort。

内置解决方案名为`sort()`，在`java.util.Arrays`类中有许多不同版本（15+种）。其中两种版本具有以下签名：

- `void sort(Object[] a)`
- `<T> void sort(T[] a, Comparator<? super T> c)`

如果我们将字符串（单词）转换为char []，那么我们可以对其字符进行排序，并通过以下辅助方法返回新的字符串：

```
// helper method for sorting the chars of a word
private static String sortWordChars(String word) {

    char[] wordToChar = word.toCharArray();
    Arrays.sort(wordToChar);

    return String.valueOf(wordToChar);
}
```

接下来，我们只需要一个Comparator，它表明互为变位词的两个字符串是等价的：

```
public class Anagrams implements Comparator<String> {

    @Override
    public int compare(String s1, String s2) {
        return sortStringChars(s1).compareTo(sortStringChars(s2));
    }
}
```

最后，我们通过这个compareTo()方法对给定的字符串（单词）数组进行排序：

```
Arrays.sort(words, new Anagrams());
```

然而，这个问题实际上并不要求我们对给定的变位词数组进行排序；问题要求我们将变位词分组打印。为此，我们可以依赖哈希（如果你不熟悉哈希的概念，请阅读第6章“面向对象编程”中的“哈希表”问题）。在Java中，我们可以通过内置的HashMap实现来使用哈希，因此无需从头编写哈希实现。那么这个HashMap有什么用处呢？我们应该在这个映射的条目（键值对）中存储什么？

每一组变位词都会收敛到相同的排序版本（例如，包含字符串（单词）"slat"、"salt"和"last"的变位词组具有唯一且共同的排序版本"alst"）。由于排序版本是唯一的，因此它是我们映射中键的良好候选。接下来，值表示变位词列表。因此，算法非常简单；它包含以下步骤：

1. 遍历给定的单词数组。
2. 对每个单词的字符进行排序。
3. 填充映射（添加或更新映射）。
4. 打印结果。

代码如下：

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

如果n是字符串（单词）的数量，且每个字符串（单词）最多有m个字符，那么上述两种方法的时间复杂度为O(nm log m)。

我们能做得更好吗？要做得更好，我们必须识别上述两种方法的问题。问题在于我们对每个字符串（单词）进行排序，这会花费额外的时间。然而，我们可以使用一个额外的char []来统计字符串（单词）中每个字符出现的次数（频率）。在构造好这个char []后，我们将其转换为String，从而得到必须在HashMap中搜索的键。由于Java处理char类型的方式与处理（无符号）short相同，我们可以用char进行计算。让我们看看代码（wordToChar数组跟踪给定数组中每个字符串（单词）中从a到z的字符频率）：

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

如果*n*是字符串（单词）的数量，且每个字符串（单词）最多包含*m*个字符，那么上述两种方法的时间复杂度为O(nm)。如果你需要支持更多字符，而不仅仅是*a*到*z*，那么请使用`int []`数组和`codePointAt()`方法——更多细节见第10章“数组与字符串”中的“提取代理对代码点”编程挑战。完整的应用程序名为*GroupSortAnagrams*。

## 编程挑战3——未知大小的列表

**问题：** 假设你有一个数据结构，表示一个未知大小的有序列表（这意味着没有`size()`或类似的方法），且只包含正数。该列表的代码如下：

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

然而，正如你所见，有一个名为peekAt()的方法，它能在O(1)时间内返回给定索引处的元素。如果给定索引超出了此列表的边界，那么peekAt()返回-1。请编写一段代码，返回元素p出现的索引。

**解法：** 当我们必须搜索一个有序的数据结构（例如，有序数组）时，我们知道二分查找是正确的选择。那么，在这种情况下我们能用二分查找吗？由于给定的列表是有序的，并且我们可以在O(1)时间内访问其任何元素，这意味着二分查找应该是一个不错的选择。除此之外，二分查找算法需要数据结构的大小，因为我们必须将搜索空间减半（例如，list.size()/2）来找到中点。而给定的数据结构（列表）没有暴露其大小。

因此，问题就简化为找到这个列表的大小。我们知道，如果给定索引超出列表边界，peekAt()会返回-1，所以我们可以循环遍历列表并计算迭代次数，直到peekAt()返回-1。当peekAt()返回-1时，我们应该知道列表的大小，从而可以应用二分查找算法。与其逐个元素地循环列表（线性算法），我们不如尝试指数级地循环。因此，与其循环peekAt(1)、peekAt(2)、peekAt(3)、peekAt(4)……，我们改为循环peekAt(1)、peekAt(2)、peekAt(4)、peekAt(8)……。换句话说，与其以O(n)时间完成，我们可以以O(log n)时间完成，其中n是列表的大小。我们能这样做是因为给定的列表是有序的！

下面的代码应该能阐明这种方法以及其余细节：

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
