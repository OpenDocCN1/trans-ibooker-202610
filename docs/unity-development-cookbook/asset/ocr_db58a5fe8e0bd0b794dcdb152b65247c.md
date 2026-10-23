![](bbox=[0.205, 0.09, 0.795, 0.45])

Figure 10-5. The avoider in the process of looking for a place to hide

# Discussion

Every so often, this algorithm checks to see if the target can see us. If the target can see us, the algorithm generates a collection of random points nearby and, for each point, figures out if the target can see that point. Any point that can be seen is then discarded, all remaining points are tested, and again any point that can be seen is discarded. Finally, each remaining point is tested to find the length of the path to reach it, and the shortest is selected.

![](bbox=[0.17, 0.66, 0.25, 0.74])

For better performance, you can increase the Search Cell Size to reduce the number of possible hiding spots that are tested. If you’ve still got too many points, you can also reduce the Search Area Size to reduce the range of overall possible hiding spots.

# 10.6 Building and Using a State Machine

# Problem

You want to use a state machine, which is a programming pattern that lets you manage the various possible object states.