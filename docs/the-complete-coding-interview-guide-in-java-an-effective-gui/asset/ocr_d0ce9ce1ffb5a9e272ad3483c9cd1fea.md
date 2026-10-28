If we scan the tree from the left-hand side to the right-hand side, we can identify seven vertical axes whose sums are 5, 7, 16, 35, 54, 44, and 6. At the top of the diagram, we've added the horizontal distance of each node from the root node. If we consider the root node as having the distance 0, then we can easily uniquely identify each vertical axis from the left or the right of the root by decreasing, respectively increasing 1 as, -3, -2, -1, 0 (the root), 1, 2, 3.

Each axis is uniquely identified by its distance from the root, and each axis holds the nodes that we have to sum up. If we think about the unique distance of an axis as a key and the sum of the nodes on this axis as a value, then we can intuit that this problem can be solved via hashing (if you are not familiar with the concept of hashing, then please take a look at Chapter 6, Object-Oriented Programming, the Hash table problem). In Java, we can use hashing via the built-in HashMap implementation, so there is no need to write a hashing implementation from scratch.

But how can we fill up this map? It is quite obvious that we have to traverse the tree while we are filling up the map. We can start from the root and add the key to the map as 0 (0 corresponds to the axis that contains the root) and the value as the root (21). Next, we can use recursion to go to the left-axis of the root by decreasing the distance from the root by 1. We can also use recursion to go through the right-axis of the root by increasing the distance from the root by 1. At every node, we update the value in the map that corresponds to the key that identifies the current axis. So, if we recursively follow path root|left sub-tree|right sub-tree, then we use the Pre-Order traversal of a binary tree.

In the end, our map should contain the following key-value pairs: (-3, 5), (-2, 7), (-1, 16), (0, 35), (1, 54), (2, 44), and (3, 6).

Putting this algorithm into code results in the following (map contains the vertical sums):

```
private void verticalSum(Node root,
    Map<Integer, Integer> map, int dist) {

    if (root == null) {
        return;
    }

    if (!map.containsKey(dist)) {
        map.put(dist, 0);
    }

    map.put(dist, map.get(dist) + root.element);
```