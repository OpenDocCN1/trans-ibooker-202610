![](bbox=[0.145, 0.092, 0.855, 0.568])

Figure 6-11. The same shot as Figure 6-10, but with the bloom effect enabled

# Discussion

HDR rendering is different from low-dynamic-range (LDR) rendering.
LDR clamps the value of each color channel (red, green, and blue) to
between 0 and 1, while HDR can go above or below. This means that a
much wider range of exposure levels can be accomplished, and allows for
greater flexibility when processing the image before it’s displayed.

The display that the player sees the game through has a limited color range,
which means that an HDR image must be clamped, through a process
known as tone mapping. Tone mapping involves deciding what the top and
bottom of the range of colors will be; any color level below the bottom will
appear black, and any color level above the top will appear white. Because