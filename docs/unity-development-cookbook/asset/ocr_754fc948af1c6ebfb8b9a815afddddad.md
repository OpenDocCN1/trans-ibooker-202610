# 5.10 Using a Custom Material for Sprites

# Problem

You want to use a custom material to draw your sprites.

# Solution

To use a custom material for your sprites:

+   1. Create a new material asset by opening the Assets menu and choosing Create → Material. Name the new material “Sprite Diffuse.”

+   2. Select the new sprite, and at the top of the Inspector, change the shader to Sprites → Diffuse (Figure 5-11).

+   3. Select any game object in your scene that has a sprite renderer, and drag your Sprite Diffuse material into its Material slot. It will now respond to lighting (Figure 5-12).

![](bbox=[0.14, 0.42, 0.86, 0.77])

Figure 5-11. Selecting the Sprites → Diffuse shader on a material