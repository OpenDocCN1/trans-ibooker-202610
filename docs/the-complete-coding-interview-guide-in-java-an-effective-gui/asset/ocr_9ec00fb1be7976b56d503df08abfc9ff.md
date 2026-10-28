```
flag = true;
minElement = currentElement;
} else {
// enqueue the current element which is not the minimum
queue.add(currentElement);
}
}

// enqueue the minimum element
queue.add(minElement);
}
```

This code's runtime is O(n²). The complete application is called *SortQueueWithoutExtraSpace.*

# Coding challenge 8 – Sorting a stack with the help of another stack

**Amazon, Google, Adobe, Microsoft, Flipkart**

**Problem:** Consider you’ve been given an unsorted stack. Write a snippet of code that sorts the stack in descending or ascending order. You can only use an additional temporary stack.

**Solution:** If we could use two additional stacks, then we could implement an algorithm that repeatedly searches the minimum value in the given stack and pushes it into the final or resulting stack. A second additional stack will be used as a buffer while searching the given stack. However, the problem requires us to use only one additional temporary stack.

Due to this constraint, we are forced to pop from the given stack (let’s denote it as s1) and push in order into the additional stack (let’s denote it as s2). To accomplish this, we use a temporary or auxiliary variable (let’s denote it as t), as shown in the following diagram (the given stack is top → 1 → 4 → 5 → 3 → 1 → 2):