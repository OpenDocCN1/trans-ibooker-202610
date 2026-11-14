
# Keyboard Input Implementation

## Update Function

```javascript
function update() {
    let i;
    for (i = 0; i < keys.LastKeyCode; i++) {
        mIsKeyClicked[i] = (!mKeyPreviousState[i]) && mIsKeyPressed[i];
        mKeyPreviousState[i] = mIsKeyPressed[i];
    }
}
```

## Public Inquiry Functions

Add public functions for inquires to current keyboard states to support the client game developer:

```javascript
// Function for GameEngine programmer to test if a key is pressed down
function isKeyPressed(keyCode) {
    return mIsKeyPressed[keyCode];
}

function isKeyClicked(keyCode) {
    return mIsKeyClicked[keyCode];
}
```

## Export Public Functions and Constants

Finally, export the public functions and key constants:

```javascript
export {keys, init, update, isKeyClicked, isKeyPressed};
```

# Modify the Engine to Support Keyboard Input

To properly support input, before the game loop begins, the engine must initialize the `mIsKeyPressed`, `mIsKeyClicked`, and `mKeyPreviousState` arrays. To properly capture the player actions, during gameplay from within the core of the game loop, these arrays must be updated accordingly.

## Input State Initialization (index.js)

Modify `index.js` by importing the `input.js` module, adding the initialization of the input to the engine `init()` function, and adding the input module to the exported list to allow access from the client game developer.

```javascript
import * as input from "./input.js";

function init(htmlCanvasID) {
    glSys.init(htmlCanvasID);
    vertexBuffer.init();
    shaderResources.init();
    input.init();
}

export default {
    // input support
    input,
    // Util classes
    Camera, Transform, Renderable,
    // functions
    init, clearCanvas
};
```

## Game Loop Integration (loop.js)

To accurately capture keyboard state changes, the input component must be integrated with the core of the game loop. Include the input's `update()` function in the core game loop by adding the following lines to `loop.js`. Notice the rest of the code is identical.

```javascript
import * as input from "../input.js";

function loopOnce() {
    if (mLoopRunning) {
        ... identical to previous code ...
        // Step D: update the game the appropriate number of times.
        //         Update only every kMPF (1/60 of a second)
        //         If lag larger then update frames, update until caught up.
        while ((mLagTime >= kMPF) && mLoopRunning) {
            input.update();
            mCurrentScene.update();
            mLagTime -= kMPF;
        }
    }
}
```

# Test Keyboard Input

You can test the input functionality by modifying the `Renderable` objects in your `MyGame` class. Replace the code in the `MyGame` `update()` function with the following:

```javascript
update() {
    // Simple game: move the white square and pulse the red
    let whiteXform = this.mWhiteSq.getXform();
    let deltaX = 0.05;
    // Step A: test for white square movement
    if (engine.input.isKeyPressed(engine.input.keys.Right)) {
        if (whiteXform.getXPos() > 30) { // right-bound of the window
            whiteXform.setPosition(10, 60);
        }
        whiteXform.incXPosBy(deltaX);
    }
    // Step B: test for white square rotation
    if (engine.input.isKeyClicked(engine.input.keys.Up)) {
        whiteXform.incRotationByDegree(1);
    }
    let redXform = this.mRedSq.getXform();
    // Step C: test for pulsing the red square
    if (engine.input.isKeyPressed(engine.input.keys.Down)) {
        if (redXform.getWidth() > 5) {
            redXform.setSize(2, 2);
        }
        redXform.incSizeBy(0.05);
    }
}
```

In the previous code, step A ensures that pressing and holding the right-arrow key will move the white square toward the right. Step B checks for the pressing and then the releasing of the up-arrow key event. The white square is rotated when such an event is detected. Notice that pressing and holding the up-arrow key will not generate continuously key press events and thus will not cause the white square to continuously rotate. Step C tests for the pressing and holding of the down-arrow key to pulse the red square.

You can run the project and include additional controls for manipulating the squares. For example, include support for the `WASD` keys to control the location of the red square. Notice once again that by increasing/decreasing the position change amount, you are effectively controlling the speed of the object's movement.

> **Note**
>
> The term "`WASD` keys" is used to refer to the key binding of the popular game controls: key `W` to move upward, `A` leftward, `S` downward, and `D` rightward.

# Resource Management and Asynchronous Loading

Video games typically utilize a multitude of artistic assets, or resources, including audio clips and images. The required resources to support a game can be large. Additionally, it is important to maintain the independence between the resources and the actual game such that they can be updated independently, for example, changing the background audio without changing the game itself. For these reasons, game resources are typically stored externally on a system hard drive or a server across the network. Being stored external to the game, the resources are sometimes referred to as *external resources* or *assets*.

After a game begins, external resources must be explicitly loaded. For efficient memory utilization, a game should load and unload resources dynamically based on necessity. However, loading external resources may involve input/output device operations or network packet latencies and thus can be time intensive and potentially affect real-time interactivity. For these reasons, at any instance in a game, only a portion of resources are kept in memory, where the loading operations are strategically executed to avoid interrupting the game. In most cases, resources required in each level are kept in memory during the gameplay of that level. With this approach, external resource loading can occur during level transitions where players are expecting a new game environment and are more likely to tolerate slight delays for loading.

Once loaded, a resource must be readily accessible to support interactivity. The efficient and effective management of resources is essential to any game engine. Take note of the clear differentiation between resource management, which is the responsibility of a game engine, and the actual ownerships of the resources. For example, a game engine must support the efficient loading and playing of the background music for a game, and it is the game (or client of the game engine) that actually owns and supplies the audio file for the background music. When implementing support for external resource management, it is important to remember that the actual resources are not part of the game engine.

At this point, the game engine you have been building handles only one type of resource—the GLSL shader files. Recall that the `SimpleShader` object loads and compiles the `simple_vs.glsl` and `simple_fs.glsl` files in its constructor. So far, the shader file loading has been accomplished via synchronous `XMLHttpRequest.open()`. This synchronous loading is an example of inefficient resource management because no operations can occur while the browser attempts to open and load a shader file. An efficient alternative would be to issue an asynchronous load command and allow additional operations to continue while the file is being opened and loaded.

This section builds an infrastructure to support asynchronous loading and efficient accessing of the loaded resources. Based on this infrastructure, over the next few projects, the game engine will be expanded to support batch resource loading during scene transitions.

## The Resource Map and Shader Loader Project
