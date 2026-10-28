

// if stack is empty there is no left smaller element
if (stack.empty()) {
    System.out.print("_, ");
} else {
    // the top of the stack is the left smaller element
    System.out.print(stack.peek() + ", ");
}

// push arr[i] into the stack
stack.push(arr[i]);
}
}

这段代码的运行时间为 O(n)，其中 n 是给定数组中的元素数量。完整的应用程序称为 FindNearestMinimum。

## 编程挑战 17 – 单词搜索

Amazon, Google

问题：假设你有一个 2D 棋盘（矩阵）和一个单词。棋盘的每个单元格包含一个字母。你可以通过连接水平或垂直相邻的单元格的字母来构造一个单词。编写一段代码，如果给定的单词存在于棋盘上则返回 true。相同的字母单元格不能使用超过一次。

解决方案：假设我们有以下棋盘：

![](img/6709a71136a6b327c831c6c15aff2f6a_648_0.png)

图 14.33 – 棋盘示例

请记住，这并不是我们第一次需要解决在网格中寻找特定路径的问题。在第 8 章《递归与动态规划》中，我们有机器人网格问题，包括 Color spots、Five Towers、The falling ball 和 Knight tour。在第 12 章《栈与队列》中，我们有 Islands。最后，在第 13 章《树与图》中，我们有 Chess knight。

基于你从这些问题中积累的经验，挑战自己在没有任何进一步说明的情况下为此问题编写实现。完整的应用程序称为 WordSearch。如果 k 是给定单词的长度，棋盘大小为 m x n，则该应用程序运行时间为 O(m * n * 4^k)。

## 编程挑战 18 – 基于另一个数组对数组排序

Amazon, Google, Microsoft

问题：假设你有两个数组。编写一段代码，根据第二个数组定义的顺序重新排列第一个数组的元素。

| int [] firstArr | {4, 1, 8, 1, 3, 8, 6, 7, 4, 9, 8, 2, 5, 3} |
| int [] secondArr | {7, 4, 8, 11, 2} |

期望结果是 {7, 4, 4, 8, 8, 2, 1, 1, 3, 5, 6, 9}。

该问题的解决方案依赖于哈希。更精确地说，我们可以使用以下算法：

- 1. 统计并存储第一个数组中每个元素的频率到一个映射中。
- 2. 对于第二个数组中的每个元素，检查当前元素是否在映射中。

然后，执行以下操作：
- a. 如果是，则在第一个数组中设置 n 次（n 是第二个数组中当前元素在第一个数组中的频率）。
- b. 从映射中删除当前元素，这样最后映射中只会包含存在于第一个数组中但不存在于第二个数组中的元素。

- 3. 将映射中的元素追加到第一个数组的末尾（这些元素已经排序，因为我们使用了 TreeSet）。

让我们看一下代码：

```
public static void custom(int[] firstArr, int[] secondArr) {

    // store the frequency of each element of first array
    // using a TreeMap stores the data sorted
    Map<Integer, Integer> frequencyMap = new TreeMap<>();

    for (int i = 0; i < firstArr.length; i++) {
        frequencyMap.putIfAbsent(firstArr[i], 0);
        frequencyMap.put(firstArr[i],
            frequencyMap.get(firstArr[i]) + 1);
    }

    // overwrite elements of first array
    int index = 0;

    for (int i = 0; i < secondArr.length; i++) {

        // if the current element is present in the 'frequencyMap'
        // then set it n times (n is the frequency of
        // that element in the first array)
        int n = frequencyMap.getOrDefault(secondArr[i], 0);
        while (n-- > 0) {
            firstArr[index++] = secondArr[i];
        }

        // remove the element from map
        frequencyMap.remove(secondArr[i]);
    }

    // copy the remaining elements (the elements that are
    // present in the first array but not present
    // in the second array)
    for (Map.Entry<Integer, Integer> entry :
        frequencyMap.entrySet()) {
```

```
int count = entry.getValue();
while (count-- > 0) {
firstArr[index++] = entry.getKey();
}
}
}
```

这段代码的运行时间是 O(m log m + n)，其中 m 是第一个数组中的元素数量，n 是第二个数组中的元素数量。完整的应用程序称为 SortArrayBasedOnAnotherArray。

好吧，这是本章的最后一个问题。现在，是时候总结我们的工作了！

# 总结

这是一章全面的内容，涵盖了排序和搜索算法。你看到了归并排序、快速排序、基数排序、堆排序、桶排序和二分搜索的实现。此外，本书附带的<|message|> 代码中有一个名为 SortArraysIn14Ways 的应用程序，其中包含 14 种排序算法的实现。

在下一章中，我们将涵盖一系列被归类为数学和谜题问题的题目。

# 15
数学与谜题

本章涵盖了一个在面试中备受争议的话题：数学和谜题问题。大量公司认为这类问题不应成为技术面试的一部分，而其他公司仍然认为该主题相关。

该主题包含的问题具有脑力激荡性，可能需要相当程度的数学和逻辑知识。如果你计划申请在学术领域工作的公司（数学、物理、化学等），你应该预期此类问题。然而，像 Amazon 和 Google 这样的大公司也愿意依赖此类问题。

在本章中，我们将涵盖以下主题：

- 技巧和建议
- 编程挑战

到本章结束时，你应该熟悉这类问题，并能够探索更多此类问题。

## 技术要求

本章中出现的所有代码文件都可在 GitHub 上获取，地址为 https://github.com/PacktPublishing/The-Complete-Coding-Interview-Guide-in-Java/tree/master/Chapter15。

## 技巧和建议

当你遇到一个脑筋急转弯问题时，最重要的方面是不惊慌。多次阅读问题并以系统的方式写下你的结论。必须明确识别输入、输出和应遵守的约束条件。

尝试举几个例子（输入数据样本），做一些草图，并在分析问题时与面试官保持交流。面试官并不期望你立即给出解决方案，但他们期望听到你在尝试解决问题时的思路。这样，面试官可以追踪你的思路逻辑并了解你如何处理问题。

此外，写下你在开发解决方案时注意到的任何规则或模式非常重要。随着你写下每一条陈述，你就更接近解决方案。通常，如果你从解决方案的角度来看（你知道解决方案），这类问题并不极其困难；它们只需要高度的观察力和更强的注意力。

让我们尝试一个简单的例子。两个父亲和两个儿子坐下来吃鸡蛋。他们总共吃三个鸡蛋；每个人都有一个鸡蛋。这是可能的吗？

如果你第一次看到这样的问题，你可能会认为它不合逻辑或无法解决。正常情况下你会认为文本中存在错误（可能有四个鸡蛋，而不是三个）并反复阅读。这些是对脑筋急转弯问题的最常见反应。一旦你看到解决方案，它看起来就很简单。

现在，让我们扮演面试官面前的候选人。以下段落遵循 *大声思考* 方法。

乍一看似乎很明显，如果每个人都有一个鸡蛋并且有三个鸡蛋，那么其中一个人没有鸡蛋。所以，你可能会认为答案是三个人吃一个鸡蛋（他们每人吃一个鸡蛋），而第四个人什么都不吃。但问题说两个父亲和两个儿子坐下来吃鸡蛋，所以四个人都吃鸡蛋。

要不要这样想：每个人都有一个鸡蛋，他们（四个人）总共吃三个鸡蛋，所以它并没有说每个人 *吃* 一个鸡蛋；他们只是 *有* 一个鸡蛋。也许其中一个人与另一个人分享了他的鸡蛋。嗯，这似乎不太合理！

