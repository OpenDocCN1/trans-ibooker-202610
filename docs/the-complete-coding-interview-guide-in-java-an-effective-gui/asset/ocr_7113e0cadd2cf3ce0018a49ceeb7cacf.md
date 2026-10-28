Solution: So, let's recall the In-Order traversal of a binary tree. This Depth-first Search (DFS) flavor traverses the left sub-tree, then the current node, and then the right sub-tree. Now, let's assume that we arbitrarily choose a node from a BST (let's denote it as n) and we want to find its successor (let's denote it as s) in the context of In-Order traversal.

Let's consider the following diagram as the given BST. We can use it as support for distinguishing between the possible cases:

![](bbox=[0.17, 0.31, 0.82, 0.5])

Figure 13.26 – BST sample with start and successor nodes

As shown in the preceding diagram, we denoted two main cases as (a) and (b). In case (a), the node, n, has the right sub-tree. In case (b), the node, n, doesn't contain the right sub-tree.

Case (a), exemplified in the left-hand side BST, reveals that if the node, n, has the right sub-tree, then the successor node, s, is the leftmost node of this right sub-tree. For example, for n=50, the successor node is 54.

Case (b) has two sub-cases: a simple case and a tricky case. The simple case is exemplified by the middle BST shown in the preceding diagram. When the node, n, doesn't contain the right sub-tree and n is the left child of its parent, then the successor node is this parent. For example, for n=40, the successor node is 50. This is the simple sub-case of (b).

The tricky sub-case of (b) is exemplified by the right-hand side BST shown in the preceding diagram. When the node, n, doesn't contain the right sub-tree and n is the right child of its parent, then we have to traverse upward until n becomes the left child of its parent. Once we've done that, we return this parent. For example, if n=59, then the successor node is 60.

Moreover, we must consider that if n is the last node in the traversal, then we return the root's parent, which can be null.