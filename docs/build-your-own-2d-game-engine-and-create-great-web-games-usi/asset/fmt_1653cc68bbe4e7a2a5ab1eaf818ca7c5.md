
## The Multiple Cameras Project

This project demonstrates how to represent multiple views in the game world with multiple `Camera` objects. You can see an example of this project running in Figure 7-9. The source code to this project is defined in the `chapter7/7.4.multiple_cameras` folder.

![Figure 7-9 ¶ Running the Multiple Cameras project](images/334805_2_En_7_Chapter/334805_2_En_7_Fig9_HTML.jpg)

Figure 7-9 ¶ Running the Multiple Cameras project

The controls of the project are identical to the previous project:

*   **Q key**: Initiates the positional oscillation of the Dye character and the camera shake effects.
*   **WASD keys**: Move the Dye character (the Hero object). Notice that the camera WC window updates to follow the Hero object when it attempts to move beyond 90 percent of the WC bounds.
*   **Arrow keys**: Move the Portal object. Notice that the Portal object cannot move beyond 80 percent of the WC bounds.
*   **L/R/P/H keys**: Select the Left minion, Right minion, Portal object, or Hero object to be the object in focus. The L/R keys also set the camera to focus on the Left or Right minion.
*   **N/M keys**: Zoom into or away from the center of the camera.
*   **J/K keys**: Zoom into or away while ensuring the constant relative position of the currently in-focus object. In other words, as the camera zooms, the positions of all objects will change except that of the in-focus object.

The goals of the project are as follows:

*   To understand the camera abstraction for presenting views into the game world
*   To experience working with multiple cameras in the same game level
*   To appreciate the importance of interpolation configuration for cameras with specific purposes

As in previous projects, you can find external resource files in the assets folder.

### Modify the Camera

The `Camera` object will be slightly modified to allow the drawing of the viewport with a bound. This will allow easy differentiation of camera views on the canvas.

1.  **Edit `camera_main.js` and modify the `Camera` constructor** to allow programmers to define a bound number of pixels to surround the viewport of the camera:

```javascript
constructor(wcCenter, wcWidth, viewportArray, bound) {
    this.mCameraState = new CameraState(wcCenter, wcWidth);
    this.mCameraShake = null;
    this.mViewport = [];  // [x, y, width, height]
    this.mViewportBound = 0;
    if (bound !== undefined) {
        this.mViewportBound = bound;
    }
    this.mScissorBound = [];  // use for bounds
    this.setViewport(viewportArray, this.mViewportBound);
    // Camera transform operator
    this.mCameraMatrix = mat4.create();
    // background color
    this.mBGColor = [0.8, 0.8, 0.8, 1]; // RGB and Alpha
}
```

Please refer to the `setViewport()` function that follows. By default, `bound` is assumed to be zero, and the camera will draw to the entire `mViewport`. When being nonzero, the bound number of pixels that surround the `mViewport` will be left as the background color, thereby allowing easy differentiation of multiple viewports on the canvas.

2.  **Define the `setViewport()` function**:

```javascript
setViewport(viewportArray, bound) {
    if (bound === undefined) {
        bound = this.mViewportBound;
    }
    // [x, y, width, height]
    this.mViewport[0] = viewportArray[0] + bound;
    this.mViewport[1] = viewportArray[1] + bound;
    this.mViewport[2] = viewportArray[2] - (2 * bound);
    this.mViewport[3] = viewportArray[3] - (2 * bound);
    this.mScissorBound[0] = viewportArray[0];
    this.mScissorBound[1] = viewportArray[1];
    this.mScissorBound[2] = viewportArray[2];
    this.mScissorBound[3] = viewportArray[3];
}
```

Recall that when setting the camera viewport, you invoke the `gl.scissor()` function to define an area to be cleared and the `gl.viewport()` function to identify the target area for drawing. Previously, the scissor and viewport bounds are identical, whereas in this case, notice that the actual `mViewport` bounds are the bound number of pixels smaller than the `mScissorBound`. These settings allow the `mScissorBound` to identify the area to be cleared to background color, while the `mViewport` bounds define the actual canvas area for drawing. In this way, the bound number of pixels around the viewport will remain the background color.

3.  **Define the `getViewport()` function** to return the actual bounds that are reserved for this camera. In this case, it is the `mScissorBound` instead of the potentially smaller viewport bounds.

```javascript
getViewport() {
    let out = [];
    out[0] = this.mScissorBound[0];
    out[1] = this.mScissorBound[1];
    out[2] = this.mScissorBound[2];
    out[3] = this.mScissorBound[3];
    return out;
}
```

4.  **Modify the `setViewAndCameraMatrix()` function** to bind scissor bounds with `mScissorBound` instead of the viewport bounds:

```javascript
setViewAndCameraMatrix() {
    let gl = glSys.get();
    ... identical to previous code ...
    // Step A2: set up corresponding scissor area to limit clear area
    gl.scissor(this.mScissorBound[0], // x of bottom-left corner
               this.mScissorBound[1], // y position of bottom-left corner
               this.mScissorBound[2], // width of the area to be drawn
               this.mScissorBound[3]);// height of the area to be drawn
    ... identical to previous code ...
}
```

### Testing Multiple Cameras in MyGame

The MyGame level must create multiple cameras, configure them properly, and draw each independently. For ease of demonstration, two new `Camera` objects will be created, one to focus on the Hero object and one to focus on the chasing Brain object. As in the previous examples, the implementation of the MyGame level is largely identical. In this example, some portions of the `init()`, `draw()`, and `update()` functions are modified to handle the multiple `Camera` objects and are highlighted.

1.  **Modify the `init()` function to define three Camera objects**. Both the `mHeroCam` and `mBrainCam` define a two-pixel boundary for their viewports, with the `mHeroCam` boundary defined to be gray (the background color) and with `mBrainCam` white. Notice the `mBrainCam` object’s stiff interpolation setting informing the camera interpolation to converge to new values in ten cycles.

```javascript
init() {
    // Step A: set up the cameras
    this.mCamera = new engine.Camera(
        vec2.fromValues(50, 36), // position of the camera
        100,                     // width of camera
        [0, 0, 640, 480]         // viewport (orgX, orgY, width, height)
    );
    this.mCamera.setBackgroundColor([0.8, 0.8, 0.8, 1]); // sets the background to gray
    this.mHeroCam = new engine.Camera(
        vec2.fromValues(50, 30), // update each cycle to point to hero
        20,
        [490, 330, 150, 150],
        2                        // viewport bounds
    );
    this.mHeroCam.setBackgroundColor([0.5, 0.5, 0.5, 1]);
    this.mBrainCam = new engine.Camera(
        vec2.fromValues(50, 30), // update each cycle to point to brain
        10,
        [0, 330, 150, 150],
        2                        // viewport bounds
    );
    this.mBrainCam.setBackgroundColor([1, 1, 1, 1]);
    this.mBrainCam.configLerp(0.7, 10);
    ... identical to previous code ...
}
```
