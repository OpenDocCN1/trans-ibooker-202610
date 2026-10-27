Simple enough and no sweat, right? Now, let’s say you want to make 100 peanut butter and jelly sandwiches. The obvious next step is to invite more people to help. Now, if one sandwich takes five minutes to make, and you have a five-member team to make the 100 sandwiches, it’s natural to think that it would take a total of 100 minutes to make these 100 sandwiches, assuming an equal distribution of labor and that each person makes 20 sandwiches.

![](bbox=[0.17, 0.22, 0.25, 0.295])

In this particular example, *performance* is measured by the output in terms of unit of work done (one sandwich) and the time taken for that output (average time to make one sandwich). *Scalability* is understanding how much this average time is preserved as the unit of work done increases.

However, reality could be very different in terms of how you choose to implement this with five people. Let’s take a look at two approaches:

# End-to-end execution approach

In this approach, each person follows the steps to make a single sandwich, then proceeds to make the next sandwich, until they complete five sandwiches. This is illustrated in Figure 4-2.

![](bbox=[0.175, 0.455, 0.85, 0.75])

Figure 4-2. End-to-end execution

# Assembly-line approach

In this approach, you have a division of labor where you distribute the steps to different people: the first person toasts the bread, the second person applies peanut butter, the third person applies the jam, and so on, as shown in Figure 4-3.