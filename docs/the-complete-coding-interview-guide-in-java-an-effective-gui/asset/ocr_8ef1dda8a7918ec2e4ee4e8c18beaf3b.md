```
}

// if stack is empty there is no left smaller element
if (stack.empty()) {
    System.out.print("_, ");
} else {
    // the top of the stack is the left smaller element
    System.out.print(stack.peek() + ", ");
}

// push arr[i] into the stack
stack.push(arr[i]);
}
}
```

This code has a runtime of O(n), where n is the number of elements in the given array. The complete application is called FindNearestMinimum.

# Coding challenge 17 – Word search

Amazon, Google

Problem: Consider you’ve been given a 2D board (a matrix) and a word. Each cell of the board contains a letter. You can construct a word by concatenating the letters from cells that are horizontally or vertically neighboring. Write a snippet of code that returns true if the given word is present on the board. The same letter cell may not be used more than once.

Solution: Let’s consider that we have the following board:

![](bbox=[0.22, 0.68, 0.77, 0.82])

Figure 14.33 – Board sample