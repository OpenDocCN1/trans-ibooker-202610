![](bbox=[0.146, 0.089, 0.856, 0.453])

Figure 8-13. The controller for the blend tree

# # Discussion

Blend trees work in conjunction with a parameter on the animator controller. Depending on the value of the parameter, the blend tree will blend different animation clips. In Figure 8-12, you can see a blend tree that has four clips, which allow for walking backward, standing still, walking forward, and running forward.

There are multiple types of blend trees, including 2D blend trees. These are useful for when you have two kinds of directions you want to move in (for example, forward/backward and side-to-side). An example of a 2D blend tree can be seen in Figure 8-14; this blend tree uses two parameters, one for forward and reverse motion, and one for sideways motion. When the character is applying only forward motion, the regular running animation is played; when the character is applying only sideways motion, a turning animation is played; when both are applied at once, a running-while-turning animation is played.