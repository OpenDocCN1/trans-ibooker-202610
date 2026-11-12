# Discussion

The technique we’re using here is sometimes called 9-slicing, because of the nine areas of the image that are defined by the slicing process. It’s nothing more complicated than using one single image asset with nine subsections, each with a different behavior: the corners aren’t scaled, and the rest of the image is stretched as necessary.

You may notice that there are other options for the Image component’s Image Type: Simple, Tiled, and Filled.

Simple scales the whole sprite equally. Tiled behaves similarly to the Sliced setting we’re using here, but repeats the center part rather than stretching it. Filled behaves the same as Simple, but fills the sprite from a defined origin in a defined direction, method, and amount.

> **TIP**
> 
> You can learn more about using images to style UI components in the Unity manual.

# 12.3 Animating the UI

# Problem

You want to animate parts of the user interface.

# Solution

UI animation is common in video games. Interfaces often need to draw attention to themselves, or are used to help the player feel like they’ve accomplished something special, or understand the impact of something they’ve done in the game. Unity makes animating elements of the UI as easy as animating anything else built inside Unity: pretty simple!