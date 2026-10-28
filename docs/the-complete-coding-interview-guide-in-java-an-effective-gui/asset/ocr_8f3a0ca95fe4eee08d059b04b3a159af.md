```
```java
for (int i = 0; i < heights.length; i++) {
    for (int j = i + 1; j < heights.length; j++) {
        // traverse each (i, j) pair
        maxArea = Math.max(maxArea,
            Math.min(heights[i], heights[j]) * (j - i));
    }
}

return maxArea;
}
```
```

The problem with this code is that its runtime is O(n²). A better approach consists of employing a technique known as *two-pointers*. Don't worry – it is a pretty simple technique that it is quite useful to have in your toolkit. You never know when you’ll need it!

We know that we are looking for the maximum area. Since we are talking about a rectangular area, this means that the maximum area must accommodate the best report between the *biggest width* and the *biggest height* as much as possible. The *biggest width* is from 0 to n-1 (in our example, from 0 to 8). To find the *biggest height*, we must adjust the biggest width while tracking the maximum area. For this, we can start from the biggest width, as shown in the following graph:

![](bbox=[0.258, 0.551, 0.705, 0.858])

Figure 10.22 – Area with the biggest width