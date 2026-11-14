
# Mouse Input Functionality

The main functionality to be tested includes the abilities to detect which view should receive the mouse input, react to mouse button state changes, and transform mouse-click pixel positions to the WC space. As in previous examples, the `my_game.js` implementation is largely similar to previous projects. In this case, only the `update()` function contains noteworthy changes that work with the new mouse input functionality.

## The `update()` Function

```javascript
update() {
    ... identical to previous code ...
    msg = "";
    // testing the mouse input
    if (engine.input.isButtonPressed(engine.input.eMouseButton.eLeft)) {
        msg += "[L Down]";
        if (this.mCamera.isMouseInViewport()) {
            this.mPortal.getXform().setXPos(this.mCamera.mouseWCX());
            this.mPortal.getXform().setYPos(this.mCamera.mouseWCY());
        }
    }
    if (engine.input.isButtonPressed(engine.input.eMouseButton.eMiddle)){
        if (this.mHeroCam.isMouseInViewport()) {
            this.mHero.getXform().setXPos(this.mHeroCam.mouseWCX());
            this.mHero.getXform().setYPos(this.mHeroCam.mouseWCY());
        }
    }
    if (engine.input.isButtonClicked(engine.input.eMouseButton.eRight)) {
        this.mPortal.setVisibility(false);
    }
    if (engine.input.isButtonClicked(engine.input.eMouseButton.eMiddle)){
        this.mPortal.setVisibility(true);
    }
    msg += " X=" + engine.input.getMousePosX() +
            " Y=" + engine.input.getMousePosY();
    this.mMsg.setText(msg);
}
```

The `camera.isMouseInViewport()` condition is checked when the viewport context is important, as in the case of a left mouse button click in the main camera view or a middle mouse button click in the `mHeroCam` view. This is in contrast to a right or middle mouse button click for setting the visibility of the `Portal` object. These two mouse clicks will cause execution no matter where the mouse position is.

## Testing the Implementation

You can now run the project and verify the correctness of the transformation to WC space. Click and drag with left mouse button in the main view, or middle mouse button in the `mHeroCam` view, to observe the accurate movement of the corresponding object as they follow the changing mouse position. The left or middle mouse button drag actions in the wrong views have no effect on the corresponding objects. For example, a left mouse button drag in the `mHeroCam` or `mBrainCam` view has no effect on the `Portal` object. However, notice that the right or middle mouse button click controls the visibility of the `Portal` object, independent of the location of the mouse pointer. Be aware that the browser maps the right mouse button click to a default pop-up menu. For this reason, you should avoid working with right mouse button clicks in your games.

# Summary

This chapter was about controlling and interacting with the `Camera` object. You have learned about the most common camera manipulation operations including clamping, panning, and zooming. These operations are implemented in the game engine with utility functions that map the high-level specifications to actual WC window bound parameters. The sudden, often annoying, and potentially confusing movements from camera manipulations are mitigated with the introduction of interpolation. Through the implementation of the camera shake effect, you have discovered that some movements can be modeled by simple mathematical formulations. You have also experienced the importance of effective `Camera` object abstraction in supporting multiple camera views. The last section guided you through the implementation of transforming a mouse position from the Canvas Coordinate space to the WC space.

In [Chapter 5](#334805_2_En_5_Chapter.xhtml), you found out how to represent and draw an object with a visually appealing image and control the animation of this object. In [Chapter 6](#334805_2_En_6_Chapter.xhtml), you read about how to define an abstraction to encapsulate the behaviors of an object and the fundamental support required to detect collisions between objects. This chapter was about the "directing" of these objects: what should be visible, where the focus should be, how much of the world to show, how to ensure smooth transition between foci, and how to receive input from the mouse. With these capabilities, you now have a well-rounded game engine framework that can represent and draw objects, model and manage the behaviors of the objects, and control how, where, and what objects are shown.

The following chapters will continue to examine object appearance and behavior at more advanced levels, including creating lighting and illumination effects in a 2D world and simulating and integrating behaviors based on simple classical mechanics.

# Game Design Considerations

You've learned the basics of object interaction, and it's a good time to start thinking about creating your first simple game mechanic and experimenting with the logical conditions and rules that constitute well-formed gameplay experiences. Many designers approach game creation from the top-down (meaning they start with an idea for an implementation of a specific genre like a real-time strategy, tower defense, or role-playing game), which we might expect in an industry like video games where the creators typically spend quite a bit of time as content consumers before transitioning into content makers. Game studios often reinforce this top-down design approach, assigning new staff to work under seasoned leads to learn best practices for whatever genre that particular studio works in. This has proven effective for training designers who can competently iterate on known genres, but it's not always the best path to develop well-rounded creators who can design entirely new systems and mechanics from the ground-up.

## What Makes Gameplay Well-Formed?

The aforementioned might lead us to ask, "What makes gameplay well formed?" At a fundamental level, a game is an interactive experience where rules must be learned and applied to achieve a specified outcome; all games must meet this minimum criterion, including card, board, physical, video, and other game types. Taking it a step further, a good game is an interactive experience with rules people enjoy learning and applying to achieve an outcome they feel invested in. There's quite a bit to unpack in this brief definition, of course, but as a general rule, players will enjoy a game more when the rules are discoverable, consistent, and make logical sense and when the outcome feels like a satisfactory reward for mastering those rules. This definition applies to both individual game mechanics and entire game experiences. To use a metaphor, it can be helpful to think of game designs as being built with letters (interactions) that form words (mechanics) that form sentences (levels) that ultimately form readable content (genres). Most new designers attempt to write novels before they know the alphabet, and everyone has played games where the mechanics and levels felt at best like sentences written with poor grammar and at worst like unsatisfying, random jumbles of unintelligible letters.

## Design Exploration Strategy

Over the next several chapters, you'll learn about more advanced features in 2D game engines, including simulations of illumination and physical behaviors. You'll also be introduced to a set of design techniques enabling you to deliver a complete and well-formed game level, integrating these techniques and utilizing more of the nine elements of game design discussed in [Chapter 4](#334805_2_En_4_Chapter.xhtml) in an intentional way and working from the ground-up to deliver a unified experience. In the earliest stages of design exploration, it's often helpful to focus only on creating and refining the basic game mechanics and interaction model; at this stage, try to avoid thinking about setting, meta-game, systems design, and the like (these will be folded into the design as it progresses).
