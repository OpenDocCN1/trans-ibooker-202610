1. Open the GameObject menu, and select UI → EventSystem. A new EventSystem object will be created.

TIP

If you create a Canvas, you'll also create an EventSystem at the same time, because the UI system relies on the same techniques as we're using in this recipe.

2. Next, create a raycaster component that the Event System will use to determine what objects are under the cursor. Raycasters trace lines from the camera position through the cursor position and into the scene to detect what a ray hits. If the ray hits a collider, then the Event System dispatches events to the target object.

3. Select the main Camera object.

4. Open the Component menu, and choose Event → Physics Raycaster. (You can also add it by clicking the Add Component button at the bottom of the Inspector, and choosing Event → Physics Raycaster.)

NOTE

In this recipe, we use 3D objects by utilizing the 3D physics system. For a 2D game, the same techniques apply, but you'll add a Physics 2D Raycaster to your camera instead of a Physics Raycaster. The Physics Raycaster detects only 3D colliders, and the Physics 2D Raycaster detects only 2D colliders. If you're using both collider types in your scene, both of them can be on your camera at the same time.

5. Next, we'll write a script that detects when a cursor moves over the object that the script is attached to and changes the color of its renderer. Create a new C# script called ObjectMouseInteraction.cs.

6. Add the following using directive to the top of the class:

```
using UnityEngine.EventSystems;
```