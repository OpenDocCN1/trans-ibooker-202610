```
&& isIsomorphic(treeOne.right, treeTwo.left)
|| isIsomorphic(treeOne.left, treeTwo.left)
&& isIsomorphic(treeOne.right, treeTwo.right));
}
.
```

The complete application is called TwoBinaryTreesAreIsomorphic.

# Coding challenge 17 – Binary tree right view

Amazon, Google, Adobe, Microsoft, Flipkart

Problem: Consider you've been given a binary tree. Write a snippet of code that prints the right view of this tree. Printing the right view means printing all the nodes that you can see if you look at the binary tree from its right-hand side.

Solution: If you're unsure what the right view of a binary tree is, then clarify this with the interviewer. For example, the following diagram highlights the nodes that represent the right view of a binary tree:

![](bbox=[0.32, 0.47, 0.7, 0.75])

Figure 13.38 – Right view of a binary tree

So, if you are placed in the right-hand side of this tree, you'll only see nodes 40, 45, 44, 9, and 2. If we think about Level-Order traversal (BFS), we obtain the following output:

+   - 40, 47, 45, 11, 3, 44, 7, 5, 9, 2