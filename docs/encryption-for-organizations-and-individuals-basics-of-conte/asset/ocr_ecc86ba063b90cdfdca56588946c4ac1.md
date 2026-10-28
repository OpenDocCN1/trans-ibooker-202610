# ShiftRows (And a Primer on Bitwise Shift Operators)

Next, we’ll be shifting the 16 byte blocks in the 4 x 4 state grid to the left by specific amounts. In AES the first row isn’t manipulated in this step. However, each block in the second row is shifted one place to the left. The third row is offset by two and the fourth row is offset by three places, all to the left. See Figure 3-2 for a visual demonstration of this phase. Naturally, blocks shifted out of the state’s left periphery don’t disappear; instead, they reappear on its right side.

![](bbox=[0.129, 0.349, 0.909, 0.571])

**Figure 3-2.** *The shift rows step in AES. Notice how the first row isn’t altered*

These types of operators are called *bitwise shift operators* in the world of computer science. They are used to shift binary data not only to the left, but also to the right. Time for an example. Note: Left and right bit shifts are often denoted with << and >>, respectively.

```
0010 << 2 → 1000
0010 >> 1 → 0001
```