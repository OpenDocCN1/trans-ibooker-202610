The highlighted nodes are the ones that represent the right view. However, each of these nodes represents the rightmost node at each level in the tree. This means that we can adjust the BFS algorithm and print the last node of each level.

This is an O(n) complexity time algorithm with an auxiliary O(n) space (represented by the queue), where n is the number of nodes in the tree:

```
private void printRightViewIterative(Node root) {
    if (root == null) {
        return;
    }

    // enqueue root node
    Queue<Node> queue = new ArrayDeque<>();
    queue.add(root);

    Node currentNode;
    while (!queue.isEmpty()) {

        // number of nodes in the current level is the queue size
        int size = queue.size();

        int i = 0;

        // traverse each node of the current level and enqueue its
        // non-empty left and right child
        while (i < size) {

            i++;
            currentNode = queue.poll();

            // if this is last node of current level just print it
            if (i == size) {
                System.out.print(currentNode.element + " ");
            }

            if (currentNode.left != null) {
```