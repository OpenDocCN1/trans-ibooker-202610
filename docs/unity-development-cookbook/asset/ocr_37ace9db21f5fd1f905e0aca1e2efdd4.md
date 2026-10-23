+   3. Adjust the minimum and maximum points in the Inspector so that they cover the area that you want the camera to move around in.
+   4. Play the game. The camera will move when you press the arrow keys, and won’t leave the red rectangle. If the camera is outside the rectangle before the game starts, it will move inside it.

![](bbox=[0.13, 0.2, 0.84, 0.53])

Figure 9-3. Positioning a top-down 2D camera above an example game level

# Discussion

There’s not much to say here. In each frame, we check how much the player wants to move the camera, figure out how much we need to move in a frame, calculate a new position within our allowed bounds, and move the camera there. Our top-down camera system, again, is only one possible way of implementing this sort of behavior! You could also use a more advanced system, such as Cinemachine.

![](bbox=[0.15, 0.71, 0.24, 0.8])

We use a gizmo as an aid in figuring out what’s going on in the Scene view. Gizmos are elements in the Scene view that help you navigate it or understand what’s going on.