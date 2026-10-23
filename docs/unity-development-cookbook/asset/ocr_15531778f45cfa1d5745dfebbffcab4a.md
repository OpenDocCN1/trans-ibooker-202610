Each animation clip is effectively a recording of the different parameters you'd like each property being animated to be, over time.

# 8.1 Animating an Object

# Problem

You want to give something a single animation.

# Solution

Quick, single animations are all over the place in video games, and learning to give a single object a simple animation is a great way to begin to understand Unity's animation system. We're going to create a cube, and give it a simple bounce animation:

+   1. Create a new cube by opening the GameObject menu, and choosing 3D Object → Cube.

+   2. Create a new animator controller asset by opening the Assets menu and choosing Create → Animator Controller.

+   3. Add an Animator component to the cube, either by using the Component menu (Component → Miscellaneous → Animator) or by dragging and dropping the animator controller asset onto the cube in the hierarchy.

+   4. Open the Animator tab by opening the Window menu and choosing Animator.

You’ll often find that you don’t need to use any code to animate things in Unity!

+   5. Create a new animation asset by opening the Assets menu and choosing Create → Animation. Name the new asset “Bounce.”

+   6. Drag the Bounce animation into the Animator component. This will create a new animation state that uses the Bounce animation. Because it’s the first state, it will be marked as the default state (that is, when the game starts, the animator controller will start in the Bounce state). The graph will now look like Figure 8-1.