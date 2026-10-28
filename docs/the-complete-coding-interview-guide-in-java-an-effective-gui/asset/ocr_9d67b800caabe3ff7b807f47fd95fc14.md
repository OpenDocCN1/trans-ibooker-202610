```
int base, int[] cache) {
    if (base < boxes.size() && cache[base] > 0) {
        return cache[base];
    }

    Box current = boxes.get(base);

    int highest = 0;
    // since the boxes are sorted we don’t
    // look in [0, base + 1)
    for (int i = base + 1; i < boxes.size(); i++) {
        if (boxes.get(i).canBeNext(current)) {

            int height = buildMemoization(boxes, i, cache);
            highest = Math.max(height, highest);
        }
    }

    highest = highest + current.getHeight();
    cache[base] = highest;

    return highest;
}
```

The complete application is called *HighestColoredTower*. The code also contains the plain recursion approach to this problem (without *Memoization*).

# Coding challenge 10 – String permutations

**Amazon, Google, Adobe, Microsoft, Flipkart**

**Problem:** Write an algorithm that computes all the permutations of a string and accommodates the following two conditions:

+   - The given string can contain duplicates.
+   - The returned list of permutations should not contain duplicates.