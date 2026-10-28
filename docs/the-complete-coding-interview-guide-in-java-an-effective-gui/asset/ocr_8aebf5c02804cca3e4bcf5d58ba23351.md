Deleting a node follows similar logic. If we delete a duplicate node, then we just decrease its counter by one. If the counter is already equal to 1, then we just delete the node. The relevant code is as follows:

```
private Node delete(Node node, T element) {

    if (node == null) {
        return null;
    }

    if (element.compareTo(node.element) < 0) {
        node.left = delete(node.left, element);
    } else if (element.compareTo(node.element) > 0) {
        node.right = delete(node.right, element);
    }

    if (element.compareTo(node.element) == 0) {

        // START: Handle deleting duplicates
        if (node.count > 1) {
            node.count--;
            return node;
        }
        // END: Handle deleting duplicates
        ...
    }
}
```

The complete application is called BinarySearchTreeDuplicates. Another solution to this problem consists of using a hash table to keep count of the nodes. This way, you don't modify the tree structure. Challenge yourself and complete this implementation.

# Coding challenge 16 – Isomorphism of binary trees

Amazon, Google, Microsoft

Problem: Consider you've been given two binary trees. Write a snippet of code that decides whether these two binary trees are isomorphic to each other.