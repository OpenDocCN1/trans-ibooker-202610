However, let's try another perspective of thought. For each day, we want to find a prior day that has a bigger price than the current day. In other words, we are looking for the last price that is bigger than the price of the current day.

Here, we should choose a LIFO data structure that allows us to push the prices in descending order and pop the last pushed price. Once we have this in place, we can go through each day and compare the price at the top of the stack with the current day's price. Until the price on top of the stack is less than the current day's price, we can pop from the stack. But if the price at the top of the stack is bigger than the price of the current day, then we compute the stock span of the current day as the difference in days between the current day and the day for the price on top of the stack. This will work if we push the prices into the stack in descending order – the biggest price is at the top of the stack. However, since we can compute the stock span as the difference between the index of the current day and the index of the day corresponding to the last biggest stock price (let's denote it with i), we can simply store the i index in the stack; stackPrices[i] (let's denote the prices array as stackPrices) will return the price of the stock on the iᵗʰ day.

This can be accomplished by the following algorithm:

+   1. The first day has a stock span of 1 and an index of 0 – we push this index into the stack (let's denote it as dayStack; therefore, dayStack.push(0)).
+   2. We loop the remaining days (day 2 has index 1, day 3 has index 2, and so on) and do the following:
+      a. While stockPrices[i] > stockPrices[dayStack.peek()] and !dayStack.empty(), we pop from the stack (dayStack.pop()).
+   3. If dayStack.empty(), then the stock span in i+1.
+   4. If stockPrices[i] <= stockPrices[dayStack.peek()], then the stock span is i - dayStack.peek().
+   5. Push the index of the current day, i, into the stack (dayStack).

Let's see how this algorithm works for our test case:

+   1. The first day has a stock span of 1 and an index of 0 – we push this index into the stack, dayStack.push(0).
+   2. For the second day, stockPrices[1]=34 and stockPrices[0]=55. Since 34 < 55, the stock span of day 2 is i - dayStack.peek() = 1 - 0 = 1. We push in stack 1, dayStack.push(1).