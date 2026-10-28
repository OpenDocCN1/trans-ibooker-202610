The solution can be easily implemented via a Stack and the following algorithm:

+   1. Iterate the given number from left to right, digit by digit.
+      a. While the given k is greater than 0, the stack is not empty and the top element in the stack is greater than the currently traversed digit:
+         i. Pop out the top element from the stack.
+         ii. Decrement k by 1.
+      b. Push the current digit to the stack.
+   2. While the given k is greater than 0, do the following (to handle special cases such as 222222):
+      a. Pop out elements from the stack.
+      b. Decrement k by 1.

In terms of code, we have the following:

```
public static void smallestAfterRemove(String nr, int k) {

    int i = 0;
    Stack<Character> stack = new Stack<>();
    while (i < nr.length()) {
        // if the current digit is less than the previous
        // digit then discard the previous one
        while (k > 0 && !stack.isEmpty()
            && stack.peek() > nr.charAt(i)) {
            stack.pop();
            k--;
        }

        stack.push(nr.charAt(i));
        i++;
    }

    // cover corner cases such as '2222'
    while (k > 0) {
        stack.pop();
        k--;
    }
```