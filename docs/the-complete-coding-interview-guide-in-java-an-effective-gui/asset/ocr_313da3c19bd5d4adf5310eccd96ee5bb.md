```
```java
Node node = queue.poll();
position--;

// if corner node found, print it
if (position == (size - 1) || position == 0) {
    System.out.print(node.element + " ");
}

// enqueue left and right child of current node
if (node.left != null) {
    queue.add(node.left);
}

if (node.right != null) {
    queue.add(node.right);
}
}

// level done
System.out.println();
}
}
```
```

The application is called *BinaryTreePrintCorners*.

# Coding challenge 13 – Max path sum

**Amazon, Google, Adobe, Microsoft, Flipkart**

**Problem:** Consider you've been given a non-empty binary tree. Write a snippet of code that computes the maximum path sum. A path is considered any sequence of nodes starting from any node and ending in any node in the tree, along with the parent-child connections. The path must contain at least one node and may or may not go through the root of the tree.

**Solution:** The following diagram shows three examples of a max path sum: