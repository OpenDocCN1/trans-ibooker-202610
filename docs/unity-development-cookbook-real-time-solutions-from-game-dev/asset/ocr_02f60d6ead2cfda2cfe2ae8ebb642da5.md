# 6.5 Making a Material Use a Shader

# Problem

You want to control which shader a material uses.

# Solution

There are two ways you can do this.

The first option is to create a new material by opening the Assets menu, and then choosing Create → Material. Next, select your shader from the drop-down menu at the top of the Inspector.

Alternatively, if you have a shader asset that you’ve created (such as by following the steps in Recipe 6.10), select the shader in the Projects tab and create the material. The material will use the shader you have selected.

# Discussion

Material assets combine a shader with configuration data to achieve a specific material effect. When you select a shader from the drop-down menu, Unity reads the shader and determines what information it needs—what textures, colors, and other values it uses to compute the color for each pixel of an object that the material is applied to.

You can use the built-in shaders that come with Unity, or you can produce your own. We discuss creating shaders using the Shader Graph tool in several recipes in this chapter (for example, Recipes 6.10, 6.11, and 6.12).

# 6.6 Setting Up a Bloom Effect Using Post-Processing with the Built-In Render Pipeline

# Problem