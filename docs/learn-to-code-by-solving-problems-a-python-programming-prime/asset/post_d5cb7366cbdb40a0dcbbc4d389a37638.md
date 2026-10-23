
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

only 20² = 400 to raise it to height 100. This shows that 101–118 cannot be the best range to use. There’s no point trying it.

Similar logic explains why it’s pointless to try any higher range such as 102–119, 103–120, and so on. We can always slide these ranges down to make them cost less.

In summary, we are going to test exactly 101 ranges: 0–17, 1–18, 2–19, and so on, all the way up to 100–117. We’ll remember the cost of the best one. Let’s do this!

## Solving the Problem

We’ll take the solution in two steps, just as we did when solving Lifeguards. We’ll start with a function to determine the cost of a single range. Then we’ll write a main program to call this function once for each range.

## Determining the Cost of One Range

Listing 9-3 gives the code for the function that determines the cost of a given range.

```
MAX_DIFFERENCE = 17
MAX_HEIGHT = 100

def cost_for_range(heights, low, high):
    """
    heights is a list of hill heights.
    low is an integer giving the low end of the range.
    high is an integer giving the high end of a range.

    Return the cost of changing all heights of hills to be
    between low and high.
    """
    cost = 0
    for height in heights:
        if height < low:
            cost = cost + (low - height) ** 2
        elif height > high:
            cost = cost + (height - high) ** 2
    return cost
```

Listing 9-3: Solving for one particular range

```
```python
if result < min_cost:
    min_cost = result

output_file.write(str(min_cost) + '\n')

input_file.close()
output_file.close()
```
```

# Listing 9-4: Main program

We start by reading the number of hills and then read each height into the heights list.

We use the `min_cost` variable to remember the minimum cost that we’ve discovered so far. We set `min_cost` to the cost for range 0–17 ❶. Then, in a `range` for loop ❷, we try every other range cost, updating `min_cost` every time we find a smaller cost. When we’re done with this loop, we output the minimum cost that we found.

It’s time to submit our code to the judge. Our complete-search solution should solve the problem well under the time limit.

In the next problem, we’ll see an example where a straight complete-search solution is not efficient enough.

## CONCEPT CHECK

Here’s a proposed change to the code in Listing 9-4. Take this line:

```
```python
for low in range(1, MAX_HEIGHT + 1):
```
```

And change it to the following:

```
```python
for low in range(1, MAX_HEIGHT - MAX_DIFFERENCE + 1):
```
```

Is the code still correct?

+   A. Yes
+   B. No

> Answer: A. The last range that the code now checks is 83–100, so we have to argue that the ranges we no longer check—84–101, 85–102, and so on—don’t matter.
> 
> Consider the range 84–101. If we can argue that the range 83–100 is at least as good as 84–101, then we would have no reason to check range 84–101.
> 
> The range 84–101 includes height 101. But that’s pointless: the highest hill has height 100, so height 101 may as well not even be there. We can remove 101 without making the range worse. If we remove it, we’re left with the range 84–100. Aha—but 100−84 is only 16, and we’re allowed to have a difference of 17. So we can extend the range by one on the left, giving us a range of 83–100. Surely, making the range bigger like this can’t make the range any worse. It might even make the range better, since it’s now one unit closer to any hill whose height is 83 or less.
> 
> We started with range 84–101 and showed that range 83–100 is at least as good. We can make this same argument for range 85–102, 86–103, and so on. There’s no point going any higher than 83–100!

Before continuing, you might like to try solving exercises 1 and 2 from “Chapter Exercises” on page 263.

## Problem #23: Cow Baseball

To end this chapter, I’ve chosen a problem where we’ll need to bump up our algorithm design skills beyond complete search. As you read the problem, notice that there’s not all that much input. That generally signals the effectiveness of a complete-search algorithm. But not this time, because of the amount of searching that such an algorithm has to do through this input. The difficulty boils down to having too many nested loops. Why do the nested loops bite us here? What can we do about it? Read on!

This is USACO 2013 December Bronze Contest problem Cow Baseball.

## The Challenge

Farmer John has n cows. They are standing in a row, each at a distinct integer position. They are having fun throwing a baseball around.

Farmer John is watching the antics. He observes that cow x throws the ball to some cow y to its right, and then that cow y throws the ball to some cow z to its right. He also knows that the distance of the second throw is at least the distance of the first throw and at most twice the distance of the first throw. (For example, if the first throw is distance 5, then the second throw is at least distance 5 and at most distance 10.)

Determine the number of (x, y, z) triples of cows that satisfy Farmer John’s observations.

## Input

Read input from the file named baseball.in.

The input consists of the following lines:

+   - A line containing n, the number of cows. n is between 3 and 1,000.
+   - n lines, each of which gives the position of a cow. All positions are unique, and each is between 1 and 100,000,000.

## Output

Write output to the file named baseball.out.

Output the number of triples of cows that satisfy Farmer John’s observations.

The time limit for solving each test case is four seconds.

## Using Three Nested Loops

We can use three nested loops to consider all possible triples. We’ll start by looking at the code and then discuss its efficiency.

## The Code

In “Nesting” in Chapter 3, we learned that we can loop through all pairs of values using two nested loops. Doing so looks like this:

```
>>> lst = [1, 9]
>>> for num1 in lst:
```
