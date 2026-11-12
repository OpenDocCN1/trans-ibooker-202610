1. Select the camera you want to apply post-processing effects to (typically, your main camera).

2. Add a new Post Process Layer component to it.

3. Set the Layer property to PostProcessing (Figure 6-8).

![](bbox=[0.15, 0.225, 0.85, 0.545])

Figure 6-8. Configuring a Post Process Layer component to use the PostProcessing object layer

A bloom effect appears: bright parts of the screen will appear brighter by “bleeding” out to nearby parts of the screen.

# Discussion

A _post-processing effect_ is a visual effect performed on the final or near-final image that the renderer has produced. It works by taking the rendered image, putting it on a rectangle, and then running a shader on it; the shaded result is then displayed on the screen or sent to the next post-processing effect.

There are many different post-processing effects; for example, you can adjust the color curves of the final image, apply tints, modify the exposure