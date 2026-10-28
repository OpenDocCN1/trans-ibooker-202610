```
return "0";
}

String result = "";
while (i >= 0) {
    result += (c[i--]);
}

return result;
}
```

The complete application is called *MultiplyLargeNumbers.*

# Coding challenge 10 – Next greatest number with the same digits

**Amazon, Google, Microsoft**

**Problem**: Consider you've been given a positive integer. Write a snippet of code that returns the next greatest number with the same digits.

**Solution**: The solution to this problem can be observed via several examples. Let's consider the following examples:

+   - Example 1: 6 → Not possible
+   - Example 2: 1234 → 1243
+   - Example 3: 1232 → 1322
+   - Example 4: 321 → Not possible
+   - Example 5: 621873 → 623178

From the preceding examples we can intuit that the solution can be obtained by rearranging the digits of the given number. So, if we can find the set of rules for swapping the digits that leads us to the searched number, then we can attempt an implementation.