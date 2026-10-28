# **Solution:** Reversing a string using a stack can be done as follows:

+   1. Loop the string from left to right and push each character into the stack.
+   2. Loop the stack and pop the characters one by one. Each popped character is put back into the string.

The code based on these two steps is as follows:

```
```java
public static String reverse(String str) {
    Stack<Character> stack = new Stack<>();
    // push characters of the string into the stack
    char[] chars = str.toCharArray();
    for (char c : chars) {
        stack.push(c);
    }
    // pop all characters from the stack and
    // put them back to the input string
    for (int i = 0; i < str.length(); i++) {
        chars[i] = stack.pop();
    }
    // return the string
    return new String(chars);
}
```
```

The complete application is called StackReverseString.

# **Coding challenge 2 – Stack of curly braces**

Amazon, Google, Adobe, Microsoft, Flipkart

**Problem:** Consider you've been given a String containing curly braces. Write a snippet of code that returns true if there are matching pairs of curly braces. If we can find a closing curly brace for an opening one in the proper order, then we can say that we have a matching pair. For example, a string containing matching pairs looks like this: {}{}{}{}.