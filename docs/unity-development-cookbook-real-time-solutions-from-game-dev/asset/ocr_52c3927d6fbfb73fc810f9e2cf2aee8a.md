When you increase the Dissolve Amount value, parts of the object will disappear.

The next steps create the glowing edge effect:

+   1. Create an Add node (Math → Basic → Add).
+   2. Connect the Edge Thickness and Dissolve Amount properties to it.
+   3. Create a Step node (Math → Round → Step).
+   4. Connect the Simple Noise’s Out slot to the Step’s Edge input.
+   5. Connect the Add node’s Out slot to the Step’s In input.

If you were to connect the Step’s out slot to the Fragment node’s Emission input, you’d see a white edge around the dissolved parts of the object. To provide control over the color, we’ll multiply this edge with a color:

+   1. Create a new Multiply node.
+   2. Connect its output to the Emission slot of the Fragment node, and the Step and Edge Color properties to its input.

Your dissolve shader is now complete; the finished graph should look something like Figure 6-22.