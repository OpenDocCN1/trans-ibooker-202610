+   3. Add the animation clip as a state in the animator controller.
+   4. Create a new game object that has an `Animator` and a `SpriteRenderer` component.

You can test the animation by playing the game.

# Discussion

If you already have an object set up with an `Animator`, you can attach new animation states to it. We discuss how to do this in detail in Chapter 8; the only difference between 3D animations, which that chapter covers, and 2D animations is that the animation clips modify the sprite renderer's Sprite field, rather than the positions of 3D transforms.

# 5.4 Creating a Sprite with 2D Physics

# Problem

You want to add physics to your sprites that work in two dimensions.

# Solution

To create a sprite with 2D physics:

+   1. Add a game object with a sprite renderer to your scene.
+   2. Select the game object and click Add Component.
+   3. Select Physics 2D → RigidBody 2D, or type `RigidBody 2D` into the search field and select it.

Much like regular `RigidBody` objects, which work in three dimensions, `RigidBody2D` objects define the mass and position of a physical object and respond to physical forces. However, they don't define a shape on their own. To add a shape, you need to add a collider.

# Discussion

As with regular colliders, you should not have more than one 2D collider on an object. If you want a more complex shape, either use a polygon collider or create child game objects that have a different collider. The child objects don't need to have their own rigidbody.