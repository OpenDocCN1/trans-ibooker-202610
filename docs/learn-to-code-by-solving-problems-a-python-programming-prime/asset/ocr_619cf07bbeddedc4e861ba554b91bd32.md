> Answer: A. The last range that the code now checks is 83–100, so we have to argue that the ranges we no longer check—84–101, 85–102, and so on—don’t matter.
> 
> Consider the range 84–101. If we can argue that the range 83–100 is at least as good as 84–101, then we would have no reason to check range 84–101.
> 
> The range 84–101 includes height 101. But that’s pointless: the highest hill has height 100, so height 101 may as well not even be there. We can remove 101 without making the range worse. If we remove it, we’re left with the range 84–100. Aha—but 100−84 is only 16, and we’re allowed to have a difference of 17. So we can extend the range by one on the left, giving us a range of 83–100. Surely, making the range bigger like this can’t make the range any worse. It might even make the range better, since it’s now one unit closer to any hill whose height is 83 or less.
> 
> We started with range 84–101 and showed that range 83–100 is at least as good. We can make this same argument for range 85–102, 86–103, and so on. There’s no point going any higher than 83–100!

Before continuing, you might like to try solving exercises 1 and 2 from “Chapter Exercises” on page 263.

# Problem #23: Cow Baseball

To end this chapter, I’ve chosen a problem where we’ll need to bump up our algorithm design skills beyond complete search. As you read the problem, notice that there’s not all that much input. That generally signals the effectiveness of a complete-search algorithm. But not this time, because of the amount of searching that such an algorithm has to do through this input. The difficulty boils down to having too many nested loops. Why do the nested loops bite us here? What can we do about it? Read on!

This is USACO 2013 December Bronze Contest problem Cow Baseball.