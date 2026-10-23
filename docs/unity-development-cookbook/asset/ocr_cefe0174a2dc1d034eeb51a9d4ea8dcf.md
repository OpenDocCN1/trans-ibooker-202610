![](bbox=[0.14, 0.08, 0.86, 0.33])

Figure 10-3. A navigation mesh

![](bbox=[0.17, 0.38, 0.26, 0.45])

You can also select the Static checkbox, which will mark the selected objects static for all other purposes as well.

# Discussion

A navigation mesh essentially defines an area of a game environment that can be traversed by entities that have to move around the area. Typically your game world’s navigation mesh will define the parts of a scene that aren’t obstructed. For example, in a forest scene, dense parts of the forest would not be included in your navigation mesh.

You can learn how to use the navigation mesh you’ve created in Recipe 10.3.

# 10.3 Letting Entities in Your Game Follow a Path

# Problem

You want to create an object that can find a path from its current location to a given destination, avoiding walls and other obstacles. You want to test this feature by clicking different places in the world and making the object try to move to where you clicked.