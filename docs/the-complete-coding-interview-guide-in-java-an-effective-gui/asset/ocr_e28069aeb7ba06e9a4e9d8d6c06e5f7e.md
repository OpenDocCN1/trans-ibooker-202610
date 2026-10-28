```
if (temp.left != null) {
    rl.push(temp.left);
}

if (temp.right != null) {
    rl.push(temp.right);
}
}
```

The complete application is called *BinaryTreeSpiralTraversal*. In this application, you can also find the implementation based on a Deque.

# Coding challenge 21 – Nodes at a distance k from leafs

**Amazon, Google, Microsoft, Flipkart**

**Problem:** Consider you've been given a binary tree of integers and an integer, k. Write a snippet of code that prints all the nodes that are at a distance k from a leaf node.

**Solution:** We can intuit that a distance of k from a leaf means k levels above the leaf. But to clarify any doubts, let's follow the classical approach and try to visualize an example. The following diagram represents a binary tree; the highlighted nodes (40, 47, and 11) represent the nodes that are at a distance of k=2 from a leaf node:

![](bbox=[0.3, 0.65, 0.7, 0.85])

Figure 13.43 – Nodes at a distance of k=2 from a leaf node