This will add two new ports to the Fragment node, called Alpha and Alpha Clip Threshold. When the Alpha value of a fragment is below Alpha Clip Threshold, the fragment will not be rendered.

+   3. Create three properties (see Recipe 6.12): a Color called Edge Color, a Float called Dissolve Amount, and a Float called Edge Thickness. Drag all three properties out into the graph, so that each has its own node.
+   4. Select the Dissolve Amount property, and in the Node Settings tab of the Graph Inspector, set its Default value to 0.5.
+   5. Select the Edge Thickness property, and set its Default value to 0.05.
+   6. Select the Edge Color property, and set its Default value to a bright blue with an alpha value of 100.
+   7. Create a Simple Noise node (Procedural → Noise → Simple Noise). Connect its Out slot to the Alpha slot of the Fragment node, and set its Scale to 50.
+   8. Connect the Dissolve Amount to the Alpha Clip Threshold slot of the Fragment node.

When you increase the Dissolve Amount value, parts of the object will disappear.

The next steps create the glowing edge effect:

+   1. Create an Add node (Math → Basic → Add).
+   2. Connect the Edge Thickness and Dissolve Amount properties to it.
+   3. Create a Step node (Math → Round → Step).
+   4. Connect the Simple Noise’s Out slot to the Step’s Edge input.
+   5. Connect the Add node’s Out slot to the Step’s In input.

If you were to connect the Step’s out slot to the Fragment node’s Emission input, you’d see a white edge around the dissolved parts of the object. To provide control over the color, we’ll multiply this edge with a color: