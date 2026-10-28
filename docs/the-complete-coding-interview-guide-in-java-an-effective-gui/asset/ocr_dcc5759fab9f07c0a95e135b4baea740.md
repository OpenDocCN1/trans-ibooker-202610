# Figure 10.14 – Computing the median value (step 1)

Step 1 of our algorithm specifies that q[qPointer-1] <= p[pPointer] and p[pPointer-1] <= q[qPointer]. Obviously, 9 < 28, but 25 > 10, so we apply step 2 and afterward, go back to step 1. We increase qPointer and decrease pPointer, so qPointerMin becomes qPointer + 1. The new qPointer will be (4 + 6) / 2 = 5 and the new pPointer will be (7 + 9 + 1)/2 - 5 = 3. The following image will help you visualize this scenario:

# Figure 10.15 – Computing the median value (step 2)

Here, you can see that the new qPointer and new pPointer respect step 1 of our algorithm since q[qPointer-1], which is 11, is less than p[pPointer], which is, 18; and p[pPointer-1], which is 17, is less than q[qPointer], which is 65. With this, we found the perfect qPointer to be 5.