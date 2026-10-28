each node. Through this lens, we explore how foundational concepts of various algorithms and data structures can be adapted to a new type of problem.

# Binary Search Trees of Strings

When considering whether we can improve an algorithm, we should first understand the limitations of our current approach—otherwise, there’s no reason to build a more complex data structure. Therefore, before we dive into string-specific data structures, we’ll examine where binary search trees fall short when used to store strings. First, let’s see how binary search trees can be used for this search.

# Strings in Trees

Binary search trees can store not just numbers but anything sortable, from shoes (sorted by size or smell) to zombie movies (sorted by box office revenue or scariness) to food items (sorted by price, spiciness, or likelihood to cause vomiting within the next 24 hours). Binary search trees are quite versatile that way. All we need is the ability to order the items.

To store strings in a binary search tree, we can sort elements in alphabetical order. For example, each node of the binary search tree in Figure 6-1 is a single string partitioning its subtree into strings that come before and after it in the dictionary. We reuse the greater than and less than notation from binary search trees, where X < Y indicates that string X comes before string Y in the alphabet.

![](bbox=[0.21, 0.54, 0.9, 0.76])

We search binary search trees of strings just as we did for numbers. For example, to find the string LIGHT in Figure 6-1, we begin at the root node. We then compare the target value to the node’s value, using alphabetical ordering, and progress down either the left or right branch:

+   1. LIGHT > LAUGH: We proceed down the right branch.
+   2. LIGHT < ROCK: We proceed down the left branch.