# Problem

You want to allow entities in your game to find paths around a scene.

# Solution

Navigation meshes are easiest to set up when your level is made of objects that you don’t expect to move during gameplay, such as the ground, walls, and other obstacles. To define a path:

+   1. Select the objects that you don’t expect to move.
+   2. At the top right of the Inspector, click the drop-down arrow and choose Navigation Static (*Figure 10-2*).