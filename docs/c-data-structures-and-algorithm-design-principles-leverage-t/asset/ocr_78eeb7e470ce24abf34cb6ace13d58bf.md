As we can see, the number of steps is almost equal to the number of elements. Now, let's try the same thing again with a different insertion order, as shown here:

```
bst tree;
tree.insert(7);
tree.insert(5);
tree.insert(9);
tree.insert(4);
tree.insert(6);
tree.insert(10);
tree.insert(11);
tree.insert(8);
```

The BST and the steps required to find element 4 will now look as follows:

![](bbox=[0.331, 0.414, 0.637, 0.582])

As we can see, the tree is not skewed anymore. Or, in other words, the tree is balanced. The steps to find 4 have been considerably reduced with this configuration. Thus, the time complexity of **find** is not just dependent on the number of elements, but also on their configuration in the tree. If we look at the steps closely, we are always going one step toward the bottom of the tree while searching for something. And at the end, we end up at the leaf nodes (nodes without any children). Here, we return either the desired node or NULL based on the availability of the element. So, we can say that the number of steps is always less than the maximum number of levels in the BST, also known as the height of the BST. So, the actual time complexity for finding an element is O(height).

In order to optimize the time complexity, we need to optimize the height of the tree. This is also called *balancing a tree*. The idea is to reorganize the nodes after insertion/deletion to reduce the skewness of the tree. The resultant tree is called a height-balanced BST.