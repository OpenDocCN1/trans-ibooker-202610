If a has n bits and b has m bits, the sum will have at most maximum(n, m) + 1 bits, where the extra bit is for the carry. The carry bit might be 0. Expanding our diagram for a and b each having 2 bits, we get

![](bbox=[0.42, 0.19, 0.62, 0.35])

a0 and b0 are the right-most, low-order bits. For example, if a is 01, then a0 is 1. Our task is to understand what is inside that big box with the “+”. Can we break it down into calls to the 1-bit adder? Do we need to modify the 1-bit adder so it can be a component in larger circuits?

Our plan for 2-bit addition is this:

+   1. Add a0 and b0 to get s0 and a temporary carry bit c0.
+   2. Add a1, b1, and c0 to get s1 and carry bit c.

We add single bits in each step, but there is a problem in step 2: we need to add three single bits, not two. We modify the 1-bit adder to have three inputs:

![](bbox=[0.38, 0.62, 0.68, 0.75])

Now we can create our addition circuit for 2-bit numbers:

![](bbox=[0.35, 0.8, 0.72, 0.95])

I labeled the carry bits.