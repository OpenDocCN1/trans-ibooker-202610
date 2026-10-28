# 388 Linked Lists and Maps

```
} else {
            dataSet.add(currentNode.data);
            prevNode = currentNode;
        }

        currentNode = currentNode.next;
    }
```

This solution works in the time and space complexity of O(n), where n is the number of nodes in the linked list. We can try another approach that reduces space complexity to O(1). First, let's consider the following diagram as a guide for the next steps:

![](bbox=[0.1, 0.32, 0.9, 0.58])

This approach uses two pointers:

+   1. The current node, which starts from the head of the linked list and traverses the linked list, node by node, until it reaches the tail (for example, in the preceding diagram, the current node is the second node).
+   
+   2. The runner node, which starts from the same place as the current node, that is, the head of the linked list.

Additionally, the runner node iterates through the linked list and checks whether the data of each node is equal to the data of the current node. While the runner code iterates through the linked list, the current node's position remains fixed.

If the runner node detects a duplicate, then it removes it from the linked list. When the runner node reaches the tail of the linked list, the current node advances to the next node, and the runner node iterates through the linked list again starting from the current node. So, this is an O(n²) time complexity algorithm, but with an O(1) space complexity. Let's take a look at the code:

```
public void removeDuplicates() {
    Node currentNode = head;
    Node runnerNode = head;
}
```