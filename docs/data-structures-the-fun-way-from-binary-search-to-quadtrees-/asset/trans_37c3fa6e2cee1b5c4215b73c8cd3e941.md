

## 二尖瓣返流

二尖瓣返流（Mitral Regurgitation）是一种常见的心脏瓣膜疾病，指心脏收缩时血液从左心室通过未完全关闭的二尖瓣返流至左心房。

### 病因

- 二尖瓣脱垂
- 风湿性心脏病
- 感染性心内膜炎
- 先天性畸形
- 缺血性心脏病导致的乳头肌功能不全

### 症状

轻度二尖瓣返流可能无明显症状。中重度返流可出现乏力、心悸、活动耐量下降，严重时可出现呼吸困难、端坐呼吸等左心衰竭表现。

### 诊断

体格检查可于心尖区闻及收缩期吹风样杂音。超声心动图是诊断二尖瓣返流最重要的无创检查方法，可评估返流程度、病因及心功能状态。

### 治疗

- 药物治疗：血管扩张剂、利尿剂等改善症状
- 外科手术：二尖瓣修复术或瓣膜置换术
- 介入治疗：经导管缘对缘修复术（MitraClip）

each node. Through this lens, we explore how foundational concepts of various algorithms and data structures can be adapted to a new type of problem.

## 字符串的二叉搜索树

在考虑是否可以改进算法时，我们首先应该了解当前方法的局限性——否则，就没有理由构建更复杂的数据结构。因此，在深入研究特定于字符串的数据结构之前，我们将先检查二叉搜索树在用于存储字符串时存在哪些不足。首先，让我们看看二叉搜索树如何用于这种搜索。

## 树中的字符串

二叉搜索树可以存储的不只是数字，还可以存储任何可排序的东西，从鞋子（按尺寸或气味排序）到僵尸电影（按票房收入或恐怖程度排序）再到食物（按价格、辣度或在未来24小时内导致呕吐的可能性排序）。二叉搜索树就是这样多才多艺。我们只需要能够对项目进行排序。

要在二叉搜索树中存储字符串，我们可以按字母顺序对元素进行排序。例如，图6-1中二叉搜索树的每个节点都是一个单独的字符串，将其子树划分为在字典中在其之前和之后的那些字符串。我们重用了二叉搜索树中的大于和小于符号，其中 X < Y 表示字符串 X 在字母表中出现在字符串 Y 之前。

![](img/1fcca4248e2f91469117a442dc186270_101_0.png)

我们搜索字符串的二叉搜索树的方式与搜索数字时相同。例如，要在图6-1中找到字符串 LIGHT，我们从根节点开始。然后我们将目标值与节点的值进行比较，使用字母顺序，并沿着左分支或右分支向下进行：

- 1. LIGHT > LAUGH: We proceed down the right branch.
- 2. LIGHT < ROCK: We proceed down the left branch.

entire target against the value at the node. We need additional information, the placement of the character that we are checking for at this level. We can track this additional state by passing the index to check into our recursive search function and incrementing it with each level of recursion.

The trie wrapper allows us to hide both the reference to the root node and the initial counter required by the recursive function, simplifying the code seen by the trie’s users:

```
TrieSearch(Trie: trie, String: target):
    return TrieNodeSearch(tr.root, target, 0)
```

This wrapper guarantees that subsequent search function is called with a non-null node and the correct initial index.

The code for recursively searching a trie is a bit more complex than the code for searching a binary search tree, because we must deal with target values of different lengths:

```
TrieNodeSearch(TrieNode: current, String: target, Integer: index):
    ① IF index == length(target):
        IF current.is_entry:
            return current
        ELSE:
            return null

    ② Character: next_letter = target[index]
    ③ Integer: next_index = letterToIndex(next_letter)
    TrieNode: next_child = current.children[next_index]
    ④ IF next_child == null:
        return null
    ELSE:
        return TrieNodeSearch(next_child, target, index+1)
```

This code starts by checking the length of the target string against the current depth in order to determine whether the target should be located at this level ①. If the index is equal to the length of the string (and thus one past the last character in the string), the code then checks whether the current node is itself a valid entry. This check is particularly necessary when the search terminates at an internal node, since we need to confirm that this node represents a valid entry in its own right, not just the prefix of another entry.

If the code has not reached the end of the target string, it continues the search by examining the next character in our target ②. We can define a helper function to map the character to the correct index in the array ③. The code then checks whether the corresponding child exists ④. If there isn’t a corresponding child, the code returns null, confident that target is not in the trie. If there is a corresponding child, the code follows that branch.

For an example of this search procedure, consider a trie of exclamations like YIKES and ZOUNDS from a recent episode of our favorite

To check whether this week’s episode contained our all-time favorite cartoon word, ZONK, we can simply search the trie. We start at the top of the trie and take the corresponding branches for each character, as shown in Figure 6-7.

![](img/1fcca4248e2f91469117a442dc186270_109_0.png)

Figure 6-7: A search of the trie of cartoon phrases for ZONK. The shaded nodes indicate the path taken during the search.

