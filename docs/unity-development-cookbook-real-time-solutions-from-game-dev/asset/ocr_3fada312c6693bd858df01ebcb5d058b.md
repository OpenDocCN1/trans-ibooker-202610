![](bbox=[0.145, 0.09, 0.855, 0.235])

Figure 9-7. The Inspector for TriggerObjectiveOnClick

# Discussion

So that was a long one! Don't worry, the discussion won't be as long as the solution.

> **TIP**
> You can learn more about UIs in Chapter 12.

Creating a quest system here involves creating something to be a quest (the cubes), a UI to show the status of the quest, and then some actual quests. When you're building a system like this, it's important to think through the different combinations of states that the objects can be involved in. Think about what a mischievous, malicious, confused, or unlucky player might do: they might do things out of order, skip over content, or find ways to do what your code doesn't expect.

# 9.5 Dragging a Box to Select Objects

# Problem

You want to click and drag the mouse to create a visible rectangle onscreen. When you release the mouse, you want to know which objects are inside that rectangle.