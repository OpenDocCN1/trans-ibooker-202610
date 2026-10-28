```
+ element + " for " + time + " minutes !");

    return current;
}

    if (element.compareTo(current.element) < 0) {
        current.left = insert(current.left, element, time);
    } else {
        current.right = insert(current.right, element, time);
    }

    return current;
}

public void printInOrder() {
    printInOrder(root);
}

private void printInOrder(Node node) {
    if (node != null) {
        printInOrder(node.left);
        System.out.print(" " + node.element
            + "(" + node.time + ")");
        printInOrder(node.right);
    }
}
}
```

Notice that we can easily print the timeline by using the In-Order traversal of a BST. The complete application is called BinaryTreeLandingReservation.