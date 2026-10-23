# Understanding the Canvas

Since everything you built using Unity’s UI system lives inside a canvas, it pays to understand how this works as well. First and foremost, a canvas has a render mode setting, which dictates, somewhat obviously, how the canvas is to be rendered. The possible modes are: screen space overlay, screen space camera, and world space.

On the screen space side, overlay means that canvas is rendered on top of the scene. If the screen size/resolution changes, the canvas is sized to match. Screen space camera behaves fairly similarly to overlay, but in this case the canvas is placed in front of a camera (specified by you), and everything is rendered based on the settings of the camera. For a refresher on cameras, refer to Chapter 8.

Rendering the canvas in world space makes the canvas behave like a regular object in the scene: you’d use this mode for displaying a UI that’s meant to be part of the world in your game, such as a spaceship control panel, or similar. The size of the canvas, in this case, is taken from the size of its RectTransform.

# 12.2 Theming Controls

# Problem

You’re using your own artwork for your controls, rather than the built-in Unity artwork. You want this art to be able to scale as the controls change its size, without stretching.

# Solution

When you’re building game art, especially for a UI, you want to be sure your interface elements look their best at whatever resolution the game ends up running in. To do this for UI elements, you need to set the sprites you use to draw your UI in a certain way, and tell the Unity engine which bits can be scaled without looking bad:

+   1. Start by importing the textures you want to use.

![](bbox=[0.2, 0.73, 0.28, 0.81])

When working with UIs, you must ensure that the textures are configured to be imported as “Sprite / UI.” For a reminder on how to set up asset import, see Chapter 1.

For this example, we’ll customize a panel using art from the open source UI asset kit by Kenney. We’ll be using the PNG folder, and setting the textures as sprites in the importer.