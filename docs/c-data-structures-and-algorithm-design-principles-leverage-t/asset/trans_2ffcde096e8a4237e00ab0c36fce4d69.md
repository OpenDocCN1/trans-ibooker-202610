

因此，我们首先将根节点与7进行比较。如果它大于7，我们就移动到左子树，因为那里的所有元素都小于父节点，反之亦然。我们逐个比较每个子节点，直到遇到7，或者遇到一个小于7且没有右节点的节点。在这种情况下，到达节点4后就会找到我们的目标7。

正如我们所见，我们并没有遍历整棵树。相反，当当前节点不是目标节点时，我们通过选择左侧或右侧来将搜索范围缩小一半。这类似于线性结构中的二分查找，我们将在第4章*分而治之*中学习。

## 向二叉搜索树中插入新元素

现在，让我们看看插入是如何工作的。步骤如下面的图所示：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_87_0.png)

可以看到，首先，我们必须找到要插入新值的父节点。因此，我们必须采用与搜索类似的方法；也就是说，从根节点开始，通过将每个节点与新元素进行比较来决定移动方向。在最后一步中，18大于17，但17没有右子节点。因此，我们将18插入该位置。

## 从二叉搜索树中删除元素

现在，让我们看看删除是如何工作的。考虑以下二叉搜索树：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_88_0.png)

图2.7：以12为根节点的二叉搜索树

我们将删除树中的根节点12。让我们看看如何删除任意值。这比插入要棘手一些，因为我们需要找到被删除节点的替代者，以便保持二叉搜索树的属性。

第一步是找到要删除的节点。之后，有三种可能的情况：

- 该节点没有子节点：直接删除该节点。
- 该节点只有一个子节点：将父节点对应的指针指向该唯一的子节点。
- 该节点有两个子节点：在这种情况下，我们用其后继节点替换当前节点。

后继节点是当前节点之后的下一个最大的数。换句话说，后继节点是所有大于当前节点的元素中最小的那个。因此，我们首先进入右子树，其中包含所有大于当前节点的元素，并找到其中最小的元素。找到最小节点意味着尽可能向左子树移动，因为左子节点总是小于其父节点。在图2.7所示的树中，12的右子树从18开始。因此，我们从那里开始查找，然后尝试向下移动到15的左子节点。但15没有左子节点，而另一个子节点16大于15。因此，15应该是这里的后继节点。

要用15替换12，首先，我们将在根节点复制后继节点的值，同时删除12，如下图所示：

### 步骤1

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_89_0.png)

图2.8：将后继节点复制到根节点

接下来，我们需要从右子树中的原位置删除后继节点15，如下图所示：

### 步骤2

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_89_1.png)

15被其唯一的子树替换

图2.9：从原位置删除后继节点

在最后一步中，我们删除节点15。我们使用相同的过程进行此删除。由于15只有一个子节点，我们将18的左子节点替换为15的子节点。因此，以16为根节点的整个子树成为18的左子节点。

> **注意**
>
> 后继节点最多只能有一个子节点。如果它有一个左子节点，那么我们会选择那个子节点作为后继节点，而不是当前节点。

## 树上操作的时间复杂度

现在，让我们看看这些函数的时间复杂度。理论上，我们可以说每次都将搜索范围减少一半。因此，搜索具有*n*个节点的二叉搜索树所需的时间是T(n) = T(n / 2) + 1。这个方程得出的时间复杂度为T(n) = O(log n)。

但这里有一个问题。如果我们仔细看插入函数，插入的顺序实际上决定了树的形状。而且，并不一定总是像前面公式中的T(n/2)描述的那样，将搜索范围减少一半。因此，O(log n)的复杂度并不总是准确的。我们将在*平衡树*一节中更深入地探讨这个问题及其解决方案，在那里我们将看到如何更准确地计算时间复杂度。

现在，让我们在C++中实现刚才看到的操作。

## 练习9：实现二叉搜索树

在这个练习中，我们将实现*图2.7*中所示的二叉搜索树，并添加一个*find*函数来搜索元素。我们还将尝试插入和删除元素，如前面小节所述。让我们开始吧：

- 1. 首先，让我们包含所需的头文件：

```
#include <iostream>
```

- 2. 现在，让我们编写一个节点。这与我们之前的练习类似，只是我们使用整数而不是字符串：

```
struct node
{
    int data;
    node *left, *right;
};
```

- 3. 现在，让我们在节点上添加一个包装器，以提供干净的接口：

```
struct bst
{
    node* root = nullptr;
```

- 4. 在编写插入函数之前，我们需要编写**find**函数：

```
node* find(int value)
{
    return find_impl(root, value);
}

private:
node* find_impl(node* current, int value)
{
    if(!current)
    {
        std::cout << std::endl;
        return NULL;
    }
    if(current->data == value)
    {
        std::cout << "Found " << value << std::endl;
        return current;
    }
    if(value < current->data) // Value will be in the left subtree
    {
        std::cout << "Going left from " << current->data << ", ";
        return find_impl(current->left, value);
    }
    if(value > current->data) // Value will be in the right subtree
    {
        std::cout << "Going right from " << current->data << ", ";
        return find_impl(current->right, value);
    }
}
```

由于这是递归的，我们将实现放在一个单独的函数中，并将其设为私有，以防止有人直接使用它。

```
if(!start)
    return NULL;
if(value < start->data)
    start->left = delete_impl(start->left, value);
else if(value > start->data)
    start->right = delete_impl(start->right, value);
else
{
    if(!start->left)  // Either both children are absent or only left child is absent
    {
        auto tmp = start->right;
        delete start;
        return tmp;
    }
    if(!start->right)  // Only right child is absent
    {
        auto tmp = start->left;
        delete start;
        return tmp;
    }

    auto succNode = successor(start);
    start->data = succNode->data;
    // Delete the successor from right subtree, since it will always be in the right subtree
    start->right = delete_impl(start->right, succNode->data);
}
return start;
}
};
```

- 9. 让我们编写**main**函数，以便我们可以使用二叉搜索树：

```
int main()
{
    bst tree;
    tree.insert(12);
    tree.insert(10);
    tree.insert(20);
    tree.insert(8);
    tree.insert(11);
```

```
tree.insert(28);
tree.insert(4);
tree.insert(2);

std::cout << "Inorder: ";
tree.inorder();  // This will print all the elements in ascending order
std::cout << std::endl;

tree.deleteValue(12);
std::cout << "Inorder after deleting 12: ";
tree.inorder();  // This will print all the elements in ascending order
std::cout << std::endl;

if(tree.find(12))
    std::cout << "Element 12 is present in the tree" << std::endl;
else
    std::cout << "Element 12 is NOT present in the tree" << std::endl;
}
```

执行上述代码后，输出应如下所示：

```
Inorder: 2 4 8 10 11 12 15 20 28
Inorder after deleting 12: 2 4 8 10 11 15 20 28
Going left from 15, Going right from 10, Going right from 11,
Element 12 is NOT present in the tree
```

观察上述二叉搜索树的中序遍历结果。中序遍历会先访问左子树，然后访问当前节点，最后递归地访问右子树，如代码片段中的注释所示。因此，根据二叉搜索树的属性，我们会先访问所有小于当前节点的值，然后是当前节点，之后访问所有大于当前节点的值。由于这是递归进行的，我们将得到按升序排列的数据。

## 平衡树

在我们理解平衡树之前，让我们从以下插入顺序的二叉搜索树示例开始：

```
bst tree;
tree.insert(10);
tree.insert(9);
tree.insert(11);
```

