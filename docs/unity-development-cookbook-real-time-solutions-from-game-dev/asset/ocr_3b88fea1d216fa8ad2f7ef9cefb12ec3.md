![](bbox=[0.145, 0.092, 0.853, 0.388])

Figure 6-2. The Inspector for a material that uses the Unlit → Color shader

# Discussion

When a material uses an Unlit shader, it doesn’t participate in the lighting system. It won’t be shaded based on nearby light sources, and instead uses only a flat color. This can be great for when you’re not going for an accurate, photorealistic look.

# 6.4 Setting Up a Material Using Textures

# Problem

You want to use textures to control the appearance of a material.

# Solution

Most slots in a material can take a texture instead of a flat color. Generally, most modern materials use multiple textures to define their surface appearance; in addition to the albedo, they also use textures to define normals, roughness, ambient occlusion, and more.