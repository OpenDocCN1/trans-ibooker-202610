already when solving Data Plan in Chapter 3. There, we used a range for loop to loop exactly *n* times. We’ll do that here, too.

There’s one crucial difference between Data Plan and Village Neighborhood. In Data Plan, we read an integer, used it, and never referred to it again. We didn’t need to keep it around. But in Village Neighborhood, it’s not enough to see each integer just once. A village’s neighborhood depends on its left and right neighbors. Without access to those neighbors, we can’t calculate the size of the village’s neighborhood. We need to store all of the village positions for later use.

For an example of why we need to store all of the village positions, consider this test case:

```
6
20
50
4
19
15
1
```

There are six villages here. To find the size of a village’s neighborhood, we need that village’s left and right neighbors.

The first village in the input is at position 20. What’s the size of that village’s neighborhood? To answer that, we need access to all of the village positions so that we can find its left and right neighbors. Scanning through the positions, you can identify that the left neighbor is at position 19 and the right neighbor is at position 50. The size of this village’s neighborhood is therefore (20 − 19)/2 + (50 − 20)/2 = 15.5.

The second village in the input is at position 50. What’s the size of that village’s neighborhood? Again, we need to look through the positions to figure it out. This village happens to be the rightmost one, so we ignore this village’s neighborhood.

The third village in the input is at position 4. The left neighbor is at position 1, and the right neighbor is at position 15, so the size of this village’s neighborhood is (4 − 1)/2 + (15 − 4)/2 = 7.

The fourth village in the input is at position 19. The left neighbor is at position 15, and the right neighbor is at position 20, so the size of this village’s neighborhood is (19 − 15)/2 + (20 − 19)/2 = 2.5.

The only remaining village that we need to consider is at position 15. If you calculate its neighborhood size, you should get an answer of 7.5.