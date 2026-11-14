
*   **Light source**: Examines the effect of illumination from a single light source
*   **Multiple light sources**: Develop game engine infrastructure to support multiple light sources
*   **Diffuse reflection and normal maps**: Simulate light reflection from matte or diffuse surfaces
*   **Specular light and material**: Models light reflecting off shinning surfaces and reaching the camera
*   **Light source types**: Introduce illumination based on different types of light sources
*   **Shadow**: Approximates the results from light being blocked

Together, the projects in this chapter build a powerful tool for adding visual intricacy into your games. In order to properly render and display the results of illumination, the associated computation must be performed for each affected pixel. Recall that the `GLSL` fragment shader is responsible for computing the color of each pixel. In this way, each fundamental element of the `Phong` illumination model can be implemented as additional functionality to existing or new `GLSL` fragment shaders. In all projects of this chapter, you will begin by working with the `GLSL` fragment shader.

## Ambient Light

Ambient light, often referred to as background light, allows you to see objects in the environment when there are no explicit light sources. For example, in the dark of night, you can see objects in a room even though all lights are switched off. In the real world, light coming from the window, from underneath the door, or from the background illuminates the room for you. A realistic simulation of the background light illumination, often referred to as indirect illumination, is algorithmically complex and can be computationally expensive. Instead, in computer graphics and most 2D games, ambient lighting is approximated by adding a constant color, or the ambient light, to every object within the current scene or world. It is important to note that while ambient lighting can provide the desired results, it is only a rough approximation and does not mimic real-world indirect lighting.

### The Global Ambient Project

This project demonstrates how to implement ambient lighting within your scenes by defining a global ambient color and a global ambient intensity for drawing each `Renderable` object. You can see an example of this project running in Figure [8-1](#334805_2_En_8_Chapter.xhtml#Fig1). The source code of this project is located in the `chapter8/8.1.global_ambient` folder. ![Figure 8-1 ¶ Running the Global Ambient project](images/334805_2_En_8_Chapter/334805_2_En_8_Fig1_HTML.jpg)

Figure 8-1 ¶ Running the Global Ambient project

The controls of the project are as follows:

*   **Left mouse button**: Increases the global red ambient
*   **Middle mouse button**: Decreases the global red ambient
*   **Left-/right-arrow keys**: Decrease/increase the global ambient intensity

The goals of the project are as follows:

*   To experience the effects of ambient lighting
*   To understand how to implement a simple global ambient illumination across a scene
*   To refamiliarize yourself with the `SimpleShader`/`Renderable` pair structure to interface to `GLSL` shaders and the game engine

You can find the following external resources in the `assets` folder. The `fonts` folder contains the default system fonts and two texture images: `minion_sprite.png`, which defines the sprite elements for the hero and the minions, and `bg.png`, which defines the background.

#### Modifying the GLSL Shaders

A good place to start when implementing new shaders or shading functionality for the game engine is the `GLSL` shader. The `GLSL` code creation or modification allows you to implement the actual functional details, which, in turn, serves as the requirements for expanding the engine. For example, in this project, you will begin by adding ambient lighting functionality to all existing `GLSL` shaders. The support for this newly added functionality then becomes the requirements that guide the modifications to the rest of the engine. You will observe this implementation pattern for all of the examples in this chapter. Thus, to begin, integrate the global ambient into your `simple_fs.glsl`.

1.  **Modify the fragment shader `simple_fs.glsl` by defining two new uniform variables `uGlobalAmbientColor` and `uGlobalAmbientIntensity` and multiplying these variables with the `uPixelColor` when computing the final color for each pixel:**

```
precision mediump float; // Color of pixel
uniform vec4 uPixelColor;
uniform vec4 uGlobalAmbientColor;   // this is shared globally
uniform float uGlobalAmbientIntensity;   // this is shared globally
void main(void) {
    // for every pixel called sets to the user specified color
    gl_FragColor = uPixelColor * uGlobalAmbientIntensity *
                   uGlobalAmbientColor;
}
```

2.  **Similarly, modify the texture fragment shader `texture_fs.glsl` by adding the uniform variables `uGlobalAmbientColor` and `uGlobalAmbientIntensity`. Multiply these two variables with the sampled texture color to create the background lighting effect.**

```
uniform sampler2D uSampler; // Color of pixel
uniform vec4 uPixelColor;
uniform vec4 uGlobalAmbientColor;   // this is shared globally
uniform float uGlobalAmbientIntensity;   // this is shared globally
varying vec2 vTexCoord;
void main(void) {
    // texel color look up based on interpolated UV value in vTexCoord
    vec4 c = texture2D(uSampler, vec2(vTexCoord.s, vTexCoord.t));
    c = c * uGlobalAmbientIntensity * uGlobalAmbientColor;
    ... identical to previous code ...
}
```

#### Defining as Global Shared Resources

Ambient lighting affects the entire scene, and thus, the associated variables must be global and shared. In this case, the two variables, a color (ambient color) and a floating point (intensity of the color), should be globally accessible to the rest of the engine and to the clients. The `defaultResources` module is perfectly suited for this purpose. Edit the `src/engine/resources/default_resources.js` file and define the color and intensity variables, their corresponding getters and setters, and remember to export the functionality.

```
import * as font from "./font.js";
import * as map from "../core/resource_map.js";
// Global Ambient color
let mGlobalAmbientColor = [0.3, 0.3, 0.3, 1];
let mGlobalAmbientIntensity = 1;
function getGlobalAmbientIntensity() { return mGlobalAmbientIntensity; }
function setGlobalAmbientIntensity(v) { mGlobalAmbientIntensity = v; }
function getGlobalAmbientColor() { return mGlobalAmbientColor; }
function setGlobalAmbientColor(v) {
    mGlobalAmbientColor = vec4.fromValues(v[0], v[1], v[2], v[3]);
}
... identical to previous code ...
export {
    init, cleanUp,
    // default system font name: this is guaranteed to be loaded
    getDefaultFontName,
    // Global ambient: intensity and color
    getGlobalAmbientColor, setGlobalAmbientColor,
    getGlobalAmbientIntensity, setGlobalAmbientIntensity
}
```

#### Modifying SimpleShader

With global ambient color and intensity now implemented in the `GLSL` shaders, you need to modify the rest of the game engine to support the newly defined functionality. Recall that `simple_fs.glsl` is referenced by the `SimpleShader` class and that `texture_fs.glsl` is referenced by the `TextureShader` class. Since `TextureShader` is a subclass of `SimpleShader`, the newly defined `GLSL` functionality in `texture_fs.glsl` will be supported with appropriate `SimpleShader` modifications.

1.  **Modify the `simple_shader.js` file in the `src/engine/shaders` folder to import from the `defaultResources` module for accessing the global ambient light effects variables:**

```
import * as defaultResources from "../resources/default_resources.js";
```

2.  **Define two new instance variables in the constructor for storing the references or locations of the ambient color and intensity variables in the `GLSL` shader:**
