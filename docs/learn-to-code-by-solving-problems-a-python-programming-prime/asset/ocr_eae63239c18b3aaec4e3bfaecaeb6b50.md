That total is (40 − 23)² + (40 − 16)² + (40 − 2)² = 2,309. This is greater than 490, our current minimum cost, so 490 is still the cost to beat. (Remember that in this problem we’re trying to minimize Farmer John’s cost, whereas in Lifeguards we were trying to maximize coverage.)

The third hill is height 16, which gives us the range 16–33. There are two hills that are out of this range, the ones of heights 40 and 2. The total cost for this range is therefore (40 − 33)² + (16 − 2)² = 245. The new cost to beat is 245!

The fourth hill is height 2, which gives us the range 2–19. If you calculate the cost for this range, you should obtain a cost of 457.

The minimum cost we obtained using that algorithm is 245. Is 245 the answer? Are we done?

No and no! It turns out that the minimum cost is 221. There are two ranges that give us this minimum cost: 12–29 and 13–30. There is no hill whose height is 12. Similarly, there is no hill whose height is 13. We therefore can’t use hill heights as the possible low ends of ranges.

Think about what a correct complete-search algorithm could look like, one that’s guaranteed not to miss any ranges.

Here’s a plan that’s guaranteed to get us the correct answer. We start by calculating the cost for range 0–17. Then we calculate the cost for range 1–18. Then 2–19. Then 3–20. Then 4–21, and so on. We test every possible range, one by one, and remember the minimum cost that we obtain. The ranges we test have nothing to do with the heights of the hills. Since we’re testing every possible range, there’s no way we’ll miss finding the best one.

Which ranges should we test? How high should we go? Should we test the range 50–67? Yes. How about the range 71–88? Yes again. How about 115–132? No! Not that one.

The final range that we’ll check is 100–117. The reason has to do with the guarantee from the problem description that the height of any hill is at most 100.

Suppose we figure out the cost for range 101–118. Without even knowing the heights of the hills, we know for sure that none of the hills is in this range. The maximum height of a hill, after all, is 100, and our range starts at 101. Now slide our range from 101–118 down to 100–117. This 100–117 range costs less than the 101–118 range! That’s because 100 is closer to the hills than 101 is. For example, consider a hill of height 80. This hill would cost us 21² = 441 to raise it to height 101, but