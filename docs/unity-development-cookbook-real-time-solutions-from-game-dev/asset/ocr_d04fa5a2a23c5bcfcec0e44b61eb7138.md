1. Create your game objects that have the individual colliders. These can be any type of 2D collider, such as a box, polygon, or circle.

2. Create an empty game object, and make it the parent of the objects with colliders.

3. Next, add a CompositeCollider2D component to the parent.

4. Finally, select the child game objects and select the Used By Composite checkbox (Figure 5-6). When you select the parent object, its collider shape will be defined by combining the shapes of its children.

![](bbox=[0.145, 0.355, 0.855, 0.545])

Figure 5-6. Enabling the Used By Composite option on a collider

# Discussion

Composite colliders can be more efficient and also prevent problems caused by two colliders that are right next to each other. When two flat colliders are next to each other, an object sliding over them can “trip” over the point where they meet because of precision problems that affect most physics engines. This problem is solved by removing the corner completely.

Figure 5-7 shows an example of this. The top object does not use a composite collider, and contains three box colliders in a row, with the lines between them visible. The bottom object also has three box colliders in a row, but is configured to be part of a composite collider. The lines between