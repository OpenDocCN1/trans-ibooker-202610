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

The preceding code runs in O(n) time complexity and auxiliary space O(n), where *n* is the number of nodes in the tree. The complete application is called *BinaryTreeDistanceFromLeaf.*

# Coding challenge 22 – Pair for a given sum

**Amazon, Google, Adobe, Microsoft, Flipkart**

**Problem**: Consider you've been given a BST and a sum. Write a snippet of code that returns `true` if there is a pair of nodes that have this sum.