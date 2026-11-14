
This function implements the outlined shadow simulation algorithm and does not draw the actual shadow caster. Notice that the `mReceiver` object is drawn twice, in steps A and B2. Step A, the first `draw()` function, renders the `mReceiver` to the canvas as usual. Step B enables the stencil buffer for drawing where all subsequent drawings will be directed to switching on stencil buffer pixels. For this reason, the `draw()` function at step B2 uses the `ShadowReceiverShader` and switches on all pixels in the stencil buffer that corresponds to the `mReceiver` object. With the proper stencil buffer setup, in step C, the `draw()` function calls to the `mShadowCaster` will draw the corresponding shadow caster geometries only into the pixels that are covered by the receiver.

Lastly, once again, the `ShadowReceiver` object is designed for the client game developers to create shadows. So, remember to update the engine access file, `index.js`, to forward the newly defined functionality to the client.

## Updating Engine Support

With the new objects defined and engine configured, some of the existing engine classes must be modified to support the new shadow operations. The following summarizes the required changes without listing the straightforward changes. Please refer to the source code file for the actual implementation details.

*   `renderable.js`: Both the `ShadowCaster` and `ShadowReceiver` objects require the ability to swap the shaders to render the objects for shadow simulation purpose. This `swapShader()` function is best realized in the root of the `Renderable` hierarchy.
*   `light.js`: The `Light` source now defines `mCastShadow`, a boolean variable, and the associated getter and setter, indicating if the light should cast shadow.
*   `camera_main.js`: The `Camera` WC center must now be located at some z distance away. A `kCameraZ` constant is defined for this purpose and used in the `mCameraMatrix` computation in the `setViewAndCameraMatrix()` function.
*   `transform.js`: The `Transform` class must be modified to support being `cloneTo()` and the manipulation of a z-depth value.

## Testing the Shadow Algorithm

There are two important aspects to testing the shadow simulation. First, you must understand how to program and create shadow effects based on the implementation. Second, you must verify that `Renderable` objects can serve as shadow casters and receivers. The `MyGame` level test case is similar to the previous project with the exception of the shadow setup and drawing.

### Setting Up the Shadow

The proper way of setting up the shadow system is to create all `ShadowCaster` objects and then create and add to the `ShadowReceiver` objects. The `my_game_shadow.js` file defines the `_setupShadow()` function to demonstrate this.

```
MyGame.prototype._setupShadow = function () {
    // mLgtMinion has a LightRenderable
    this.mLgtMinionShadow = new engine.ShadowReceiver(this.mLgtMinion);
    this.mLgtMinionShadow.addShadowCaster(this.mIllumHero);
    this.mLgtMinionShadow.addShadowCaster(this.mLgtHero);
    // mIllumMinion has a SpriteAnimateRenderable
    this.mMinionShadow = new engine.ShadowReceiver(this.mIllumMinion);
    this.mMinionShadow.addShadowCaster(this.mIllumHero);
    this.mMinionShadow.addShadowCaster(this.mLgtHero);
    this.mMinionShadow.addShadowCaster(this.mLgtMinion);
    // mBg has a IllumRenderable
    this.mBgShadow = new engine.ShadowReceiver(this.mBg);
    this.mBgShadow.addShadowCaster(this.mLgtHero);
    this.mBgShadow.addShadowCaster(this.mIllumMinion);
    this.mBgShadow.addShadowCaster(this.mLgtMinion);
}
```

The `_setupShadow()` function is called at the end of the `MyGame.init()` function, when all other `GameObject` instances are properly created and initialized. This function demonstrates that different types of `Renderable` objects can serve as shadow receivers.

*   `LightRenderable`: `mLgtMinionShadow` is created with `mLgtMinon` as a receiver, which has a reference to a `LightRenderable` object.
*   `IllumRenderable`: `mBgShadow` and `mMinionShadow` are created with `mBg` and `mIllumMinion` being the receivers where both have references to `IllumRenderable` object.

Note that in order to observe shadow on an object, an explicit corresponding `ShadowReceiver` must be created and followed by explicitly adding `ShadowCaster` objects to the receiver. For example, `mLgtMinionShadow` defines the `mLgtMinion` object to be a receiver, where only the `mIllumHero` and `mLgtHero` will cast shadows on this object. Lastly, notice that `mLgtMinon` and `mIllumMinion` are both receivers and casters of shadows.

### Drawing the Shadow

In 2D drawings, objects are drawn by overwriting the previously drawn objects. For this reason, it is important to draw the shadow receivers and the shadow caster geometries before drawing the shadow casters. The following `my_game.draw()` function in `my_game_main.js` illustrates the important drawing order of objects:

```
draw() {
    // Clear the canvas
    engine.clearCanvas([0.9, 0.9, 0.9, 1.0]); // clear to light gray
    // Set up the camera and draw
    this.mCamera.setViewAndCameraMatrix();
    // always draw shadow receivers first!
    this.mBgShadow.draw(this.mCamera); // also draws the receiver object
    this.mMinionShadow.draw(this.mCamera);
    this.mLgtMinionShadow.draw(this.mCamera);
    this.mBlock1.draw(this.mCamera);
    this.mIllumHero.draw(this.mCamera);
    this.mBlock2.draw(this.mCamera);
    this.mLgtHero.draw(this.mCamera);
    this.mMsg.draw(this.mCamera);  // draw last
    this.mMatMsg.draw(this.mCamera);
}
```

It is important to note the draw ordering. All three shadow receivers are drawn first. Additionally, among the three receivers, the `mBgShadow` object is the actual background and thus is the first being drawn. Recall that in the definition of the `ShadowReceiver` class, the `draw()` function also draws the receiver object. For this reason, there is no need to call the `draw()` function of `mLgtMinion`, `mIllumMinion`, and `mBg` objects.

The rest of the `MyGame` level is largely similar to previous projects and is not listed here. Please refer to the source code for the details.

## Observations

You can now run the project and observe the shadows. Notice the effect of the stencil buffer where the shadow from the `mIllumHero` object is cast onto the minion and yet not on the background. Press the `WASD` keys to move both of the Hero objects. Observe how the shadows offer depth and distance cues as they move with the two hero objects. The `mLgtHero` on the right is illuminated by all four lights and thus casts many shadows. Try selecting and manipulating each of the lights, such as moving or changing the direction or switching the light on/off to observe the effects on the shadows. You can even try changing the color of the shadow (in `shadow_caster.js`) to something dramatic, such as to bright blue `[0, 0, 5, 1]`, and observe shadows that could never exist in the real world.

## Summary

This chapter guided you to develop a variation of the simple yet complete Phong illumination model for the game engine. The examples were organized to follow the three terms of the Phong illumination model: ambient, diffuse, and specular. The coverage of light sources was strategically intermixed to ensure proper illumination can be observed for every topic discussed.

The first example in this chapter on ambient illumination introduced the idea of interactively controlling and fine-tuning the color of the scene. The following two examples on light sources presented the notion that illumination, an algorithmic approach to color manipulation, can be localized and developed in the engine infrastructure for supporting the eventual Phong illumination model. The example on diffuse reflection and normal mapping was critical because it enabled illumination computation based on simple physical models and simulation of an environment in 3D.
