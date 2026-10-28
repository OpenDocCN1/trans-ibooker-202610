

```
int base, int[] cache) {
    if (base < boxes.size() && cache[base] > 0) {
        return cache[base];
    }

    Box current = boxes.get(base);

    int highest = 0;
    // since the boxes are sorted we don’t
    // look in [0, base + 1)
    for (int i = base + 1; i < boxes.size(); i++) {
        if (boxes.get(i).canBeNext(current)) {

            int height = buildMemoization(boxes, i, cache);
            highest = Math.max(height, highest);
        }
    }

    highest = highest + current.getHeight();
    cache[base] = highest;

    return highest;
}
```

完整应用名为 *HighestColoredTower*。代码中还包含了针对该问题的普通递归方法（不使用 *记忆化*）。

## 编程挑战 10 – 字符串排列

**Amazon、Google、Adobe、Microsoft、Flipkart**

**问题：** 编写一个算法，计算字符串的所有排列，并满足以下两个条件：

- 给定的字符串可能包含重复字符。
- 返回的排列列表不应包含重复项。

**解决方案：** 如同任何递归问题一样，关键在于识别不同子问题之间的关系和模式。我们马上会直观地感觉到，对包含重复字符的字符串进行排列，应该比对包含唯一字符的字符串进行排列更复杂。这意味着我们必须首先理解具有唯一字符的字符串的排列。

对字符串中的字符进行排列，最自然的方式可以遵循一个简单的模式：字符串中的每个字符都将成为字符串的第一个字符（交换它们的位置），然后使用递归调用排列所有剩余字母。让我们深入讨论一般情况。对于一个只包含一个字符的字符串，我们有单一排列：

$P(c_1)=c_1$

如果我们添加另一个字符，那么我们可以将排列表示如下：

$P(c_1c_2)=c_1c_2$ 和 $c_2c_1$

如果我们再添加一个字符，则必须使用 $c_1c_2$ 来表达这些排列。$c_1c_2c_3$ 的每个排列都表示 $c_1c_2$ 的一种顺序，如下所示：

$c_1c_2 \rightarrow c_1c_2c_3, c_1c_3c_2, c_3c_1c_2$

$c_2c_1 \rightarrow c_2c_1c_3, c_2c_3c_1, c_3c_2c_1$

让我们用 ABC 替换 $c_1c_2c_3$。接下来，我们将 P(ABC) 表示为示意图：

![](img/6709a71136a6b327c831c6c15aff2f6a_233_0.png)

图 8.13 – 对 ABC 进行排列

## 编程挑战 11 – 骑士巡游

**Amazon、Google**

**问题：** 考虑一个国际象棋棋盘（8x8 网格）。将一枚骑士放在棋盘上，并打印它的所有唯一移动。

**解决方案：** 正如你已经看到的，处理此类问题的最佳方法是拿起纸笔，画出情景草图。一图胜千言：

![](img/6709a71136a6b327c831c6c15aff2f6a_238_0.png)

图 8.14 – 骑士巡游

正如我们所见，骑士可以从 $(r, c)$ 格子移动到最多八个其他有效格子，即 $(r+2, c+1)$、$(r+1, c+2)$、$(r-1,c+2)$、$(r-2, c+1)$、$(r-2, c-1)$、$(r-1, c-2)$、$(r+1, c-2)$ 和 $(r+2, c-1)$。因此，为了获得从 1 到 64 的路径（如上图右侧所示），我们可以从给定位置开始，递归地尝试访问每个有效移动。如果当前路径不构成解，或者我们已经尝试了全部八个格子，那么我们就回溯。

为了尽可能高效，我们考虑以下几个方面：

- 我们从棋盘的一个角落开始：这样，骑士最初只能往两个方向移动，而不是八个方向。
- 我们按固定顺序检查有效格子：保持一条循环路径，将有助于我们比随机挑选更快地找到新移动。从 $(r, c)$ 出发的逆时针循环路径是 $(r+2, c+1)$、$(r+1, c+2)$、$(r-1, c+2)$、$(r-2, c+1)$、$(r-2, c-1)$、$(r-1, c-2)$、$(r+1, c-2)$ 和 $(r+2, c-1)$。

```
for (int i = 0; i < str.length(); i++) {
    if (table[i]) {
        Node node = head;
        for (int j = i; j < str.length(); j++) {
            if (node == null) {
                break;
            }
            node = node.next[str.charAt(j) - 'a'];
            // [0, i]: use our known decomposition
            // [i+1, j]: use this String in the Trie
            if (node != null && node.leaf) {
                table[j + 1] = true;
            }
        }
    }
    // table[n] would be true if
    // all characters of str can be segmented
    return table[str.length()];
}
```

显然，由于我们有两个嵌套循环，该解决方案的运行时为 O(n²)。实际上，如果节点为 null，内层循环就会中断。而在最坏情况下，这发生在 k 步之后，其中 k 是 Trie 中的最深路径。因此，对于一个包含大小为 z 的最长单词的字典，我们有 k=z+1。这意味着内层循环的时间复杂度为 O(z)，总时间复杂度为 O(nz)。额外空间为 O(Trie 的空间 + str.length)。

完整应用名为 *WordBreak*。该应用还包含一个方法，用于打印可以为给定字符串生成的所有字符串。例如，如果给定字符串是 "thisisafamousproblem"，字典是 { "this", "th", "is", "a", "famous", "f", "a", "m", "o", "u", "s", "problem" }，那么输出将包含四个序列：

- th is is a f a m o u s problem
- th is is a famous problem
- this is a f a m o u s problem
- this is a famous problem

完成！现在，是时候总结本章了。

## 总结

在本章中，我们讨论了面试中最热门的主题之一：递归和动态规划。掌握这个主题需要大量的练习。幸运的是，本章提供了一套全面的问题，涵盖了最常见的递归模式。从排列到基于网格的问题，从汉诺塔等经典问题到生成花括号等棘手问题，本章涵盖了广泛的递归案例。

不要忘记，解决递归问题的关键在于画出有意义的草图并练习多个案例。这样，你就能识别模式和递归调用。

在下一章中，我们将讨论需要位操作的问题。

因此，我们存储了 110011，这是 51 的二进制表示。其余 26 位为零（00000000 00000000 00000000 00110011）。反向过程从右到左进行，涉及在位等于 1 的位置加上 2 的幂。所以这里，51 = 2^0+2^1+2^4+2^5。下面的示意图可以帮助我们理解这一点：

![](img/6709a71136a6b327c831c6c15aff2f6a_261_0.png)

图 9.1 – 二进制转十进制（32 位整数）

在 Java 中，我们可以通过 Integer#toString(int i, int radix) 或 Integer#toBinaryString(int i) 快速查看一个数字的二进制表示。例如，基数为 2 表示二进制：

```
// 110011
System.out.println("Binary: " + Integer.toString(51, 2));
System.out.println("Binary: " + Integer.toBinaryString(51));
```

反向过程（从二进制到十进制）可以通过 Integer#parseInt(String nr, int radix) 获得：

```
System.out.println("Decimal: "
    + Integer.parseInt("110011", 2));  //51
```

接下来，让我们处理位运算符。这些运算符允许我们操作位，因此理解它们非常重要。

## 位运算符

- **一元按位取反运算符 [~]**：作为一元运算符，它需要位于数字之前的单个操作数。该运算符取数字的每一位并翻转其值，因此 1 变为 0，反之亦然；例如，5 = 101，~5 = 010。
- **按位与 [&]**：该运算符需要两个操作数，并置于两个数字之间。该运算符逐个比较两个数字的位。它类似于逻辑与 (&&)，意味着仅当比较的位都等于 1 时才返回 1；例如，5 = 101，7 = 111，5 & 7 = 101 & 111 = 101 = 5。

