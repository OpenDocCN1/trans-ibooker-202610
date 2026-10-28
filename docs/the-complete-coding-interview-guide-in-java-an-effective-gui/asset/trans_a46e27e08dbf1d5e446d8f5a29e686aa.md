

```
System.out.print(root.element + " ");
return true;
}

if (flip) {
// process left child before right child
boolean left = printLevel(root.left, level - 1, flip);
boolean right = printLevel(root.right, level - 1, flip);

return left || right;
} else {
// process right child before left child
boolean right = printLevel(root.right, level - 1, flip);
boolean left = printLevel(root.left, level - 1, flip);

return right || left;
}
}
```

这段代码的运行时间为 O(n²)，效率相当低下。我们能否更高效地实现？可以的——我们可以用额外空间 O(n) 在 O(n) 时间内通过迭代方法实现。

## 迭代方法

让我们尝试从给定图表的右侧实现螺旋顺序遍历。这次我们将通过迭代方法来实现。主要，我们可以使用两个栈（Stack）或双端队列（Deque）。让我们学习如何通过两个栈来实现。

使用两个栈的主要思想非常简单：我们使用一个栈来打印从左到右的节点，另一个栈来打印从右到左的节点。在每次迭代（或每一层）中，我们在其中一个栈中有对应的节点。在从一个栈打印节点的同时，我们将下一层的节点推入另一个栈。

以下代码将这些陈述以代码形式呈现：

```
private void printSpiralTwoStacks(Node node) {

if (node == null) {
return;
}
}
```

```
// create two stacks to store alternate levels
Stack<Node> rl = new Stack<>(); // right to left
Stack<Node> lr = new Stack<>(); // left to right

// Push first level to first stack 'rl'
rl.push(node);

// print while any of the stacks has nodes
while (!rl.empty() || !lr.empty()) {

    // print nodes of the current level from 'rl'
    // and push nodes of next level to 'lr'
    while (!rl.empty()) {

        Node temp = rl.peek();
        rl.pop();

        System.out.print(temp.element + " ");

        if (temp.right != null) {
            lr.push(temp.right);
        }

        if (temp.left != null) {
            lr.push(temp.left);
        }
    }

    // print nodes of the current level from 'lr'
    // and push nodes of next level to 'rl'
    while (!lr.empty()) {
        Node temp = lr.peek();
        lr.pop();

        System.out.print(temp.element + " ");
```

```
if (temp.left != null) {
    rl.push(temp.left);
}

if (temp.right != null) {
    rl.push(temp.right);
}
}
```

完整的应用程序称为 *BinaryTreeSpiralTraversal*。在此应用程序中，你还可以找到基于 Deque 的实现。

## 编程挑战 21 – 与叶子节点距离为 k 的节点

**Amazon, Google, Microsoft, Flipkart**

**问题：** 假设你已获得一个整数二叉树和一个整数 k。编写一段代码打印出与叶子节点距离为 k 的所有节点。

**解决方案：** 我们可以推断，与叶子节点的距离为 k 意味着在叶子节点上方 k 层。但为澄清任何疑问，让我们遵循经典方法并尝试可视化一个示例。以下图表表示一棵二叉树；突出显示的节点（40、47 和 11）表示与叶子节点距离 k=2 的节点：

![](img/6709a71136a6b327c831c6c15aff2f6a_563_0.png)

图 13.43 – 与叶子节点距离为 k=2 的节点

从前面的图表中，我们可以得出以下观察结果：

- 节点 40 与叶子 44 的距离为 2。
- 节点 47 与叶子 9 和叶子 5 的距离为 2。
- 节点 11 与叶子 2 的距离为 2。

如果我们查看每一层，则可以看到以下内容：

- 与叶子节点距离为 1 的节点是 3、11、7 和 45。
- 与叶子节点距离为 2 的节点是 11、47 和 40。
- 与叶子节点距离为 3 的节点是 40 和 47。
- 与叶子节点距离为 4 的节点是 40。

因此，根节点是距离叶子最远的节点，并且 k 不合理地大于层数；即 1。如果我们从根开始并向下遍历树直到找到叶子，则 resulting 路径应包含距离该叶子为 k 的节点。

例如，一个可能的路径是 40（根）、47、11、7 和 2（叶子）。如果 k=2，则节点 11 与叶子的距离为 2。另一个可能的路径是 40（根）、47、11 和 5（叶子）。如果 k=2，则节点 47 与叶子的距离为 2。 yet 另一个路径是 40（根）、47、3 和 9（叶子）。如果 k=2，则节点 47 与叶子的距离为 2。我们已经找到了该节点；因此，我们现在必须注意并删除重复项。

迄今为止列出的路径表明存在树的先序遍历（根 | 左子树 | 右子树）。在遍历期间，我们必须跟踪当前路径。换句话说，构造的路径由先序遍历中当前节点的祖先组成。当我们找到叶子节点时，我们必须打印距离该叶子为 k 的祖先。

为了消除重复，我们可以使用一个 Set（我们将其表示为 nodesAtDist），如以下代码所示：

```
private void leafDistance(Node node,
    List<Node> pathToLeaf, Set<Node> nodesAtDist, int dist) {

    if (node == null) {
        return;
    }

```
// for each leaf node, store the node at distance 'dist'
if (isLeaf(node) && pathToLeaf.size() >= dist) {
    nodesAtDist.add(pathToLeaf.get(pathToLeaf.size() - dist));
    
    return;
}

// add the current node into the current path
pathToLeaf.add(node);

// go to left and right subtree via recursion
leafDistance(node.left, pathToLeaf, nodesAtDist, dist);
leafDistance(node.right, pathToLeaf, nodesAtDist, dist);

// remove the current node from the current path
pathToLeaf.remove(node);
}

private boolean isLeaf(Node node) {
    return (node.left == null && node.right == null);
}
```

前面的代码运行时间为 O(n)，辅助空间为 O(n)，其中 *n* 是树中节点的数量。完整的应用程序称为 *BinaryTreeDistanceFromLeaf*。

## 编程挑战 22 – 给定和的配对

**Amazon, Google, Adobe, Microsoft, Flipkart**

**问题**：假设你已获得一棵 BST 和一个和。编写一段代码，如果存在节点对的和为此和，则返回 `true`。

解决方案：让我们考虑以下图表中所示的 BST 和 sum=74：

![](img/6709a71136a6b327c831c6c15aff2f6a_566_0.png)

图 13.44 – 和为 74 的配对包含节点 6 和 68

因此，对于 sum=74，我们可以找到配对 (6, 68)。如果 sum=89，则配对为 (43, 46)。如果 sum=99，则配对为 (50, 49)。构成配对的节点可以来自同一子树或不同子树，也可以包括根节点和叶子节点。

解决此问题的其中一个方案依赖于哈希和递归。主要，我们使用中序遍历遍历树（左子树 | 根 | 右子树），并将每个节点的元素插入到一个集合中（例如 HashSet）。此外，在将当前节点插入集合之前，我们检查（给定和 - 当前节点的元素）是否存在于集合中。如果存在，则我们找到了一个配对，因此我们停止该过程并返回 true。否则，我们将当前节点插入集合并继续此过程，直到找到配对或遍历完成。

代码如下所示：

```
public boolean findPairSum(int sum) {

    return findPairSum(root, sum, new HashSet());
}

private static boolean findPairSum(Node node,
        int sum, Set<Integer> set) {

    // base case
    if (node == null) {
        return false;
```

```
```
}
}
return false;
```
```

此代码的运行时间为 O(n)，辅助空间为 O(n)。完整的应用程序称为 BinarySearchTreeSum。

## 编程挑战 23 – 二叉树中的垂直和

Amazon, Google, Flipkart

问题：假设你已获得一棵二叉树。编写一段代码，计算此二叉树的垂直和。

解决方案：为了对该问题有一个清晰的认识，非常重要的是你绘制一个有意义的图表。使用带方格的笔记本（数学笔记本）会很有用。这很有用，因为你必须以 45 度绘制节点之间的边；否则，可能无法正确看到节点的垂直轴。通常，当我们绘制二叉树时，我们不关心节点之间的角度，但在这种情况下，这是理解问题并找到解决方案的重要方面。

以下图表是二叉树的草图。它显示了一些有用的地标，这将引导我们找到解决方案：

![](img/6709a71136a6b327c831c6c15aff2f6a_570_0.png)

图 13.45 – 二叉树中的垂直和

