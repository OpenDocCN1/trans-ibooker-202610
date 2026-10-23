+   5. Drag the material onto the sphere you added.
+   6. Select the material, and increase the Smoothness value to about 0.8. As you do, the sphere will get shiny and reflect nearby objects by sampling the view seen by the reflection probe. Figure 6-29 shows a shiny metal sphere, reflecting the room around it. Its reflections come from a reflection probe near it (not pictured). We’ve added a little texture as well, for a nicer appearance.

![](bbox=[0.14, 0.22, 0.83, 0.56])

Figure 6-29. A shiny metal sphere, reflecting the room around it

# Discussion

In real-time rendering, accurate reflections are challenging. In the real world, reflections occur when light evenly bounces off a smooth surface, which means that an in-focus image can be achieved. This means that reflections are easy in ray-tracing renderers, because they trace the path taken by light as it bounces around a scene. However, most real-time renderers don’t trace rays, mostly because of the amount of computational power required. Instead, they perform approximations: rather than bouncing a virtual photon off an object, they divide it into polygons, calculate the positions of the polygon’s points relative to the camera position, and determine the locations of the pixels that that polygon overlaps. Then, the renderer solves the rendering equation, which is one of several simplified approximations for determining the color and brightness of a point, given information about its location, material, and nearby light sources.