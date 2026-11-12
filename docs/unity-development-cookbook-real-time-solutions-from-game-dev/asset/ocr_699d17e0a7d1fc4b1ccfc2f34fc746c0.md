+   1. Start with the Glow shader that you created in Figure 6-10.

+   2. Add a Time node (Input → Basic → Time).

The Time node exposes a Sine Time output, which is the current time, in seconds, run through the Sine function (which produces a sine wave that goes from –1 to 1).

To create a fading effect that goes from 0.2 to 1 (that is, a slight glow to a full glow), we’ll use a Remap node:

+   1. Add a Remap node (Math → Range → Remap).

+   2. Set its In Min Max values to –1 and 1. (This is the range of values that it’s expecting to receive.)

+   3. Set its Out Min Max values to 0.2 and 1. (This is the range of values that it will emit.)

+   4. Connect the Sine Time output from the Time node to its In port.

+   5. Create a new Multiply node.

+   6. Connect the Remap node and the original Multiply node (the one that multiplies the Color and the Fresnel effect) into the new Multiply node.

+   7. Connect the new Multiply node into the Emission slot of the Fragment node.

When you’re done, the graph should look like Figure 6-18.