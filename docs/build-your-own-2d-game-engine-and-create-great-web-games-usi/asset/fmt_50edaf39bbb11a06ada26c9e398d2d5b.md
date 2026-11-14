
# Light Information Uniforms

```glsl
// Light information uniform
bool uLightOn;
uniform vec4 uLightColor;
uniform vec3 uLightPosition;   // in pixel space!
uniform float uLightRadius;    // in pixel space!
```

## Implementing Light Illumination in `main()`

1. **Step A**: Sample the texture color and apply the ambient color and intensity.
2. **Step B**: Perform the light source illumination. This is accomplished by determining if the computation is required—testing if the light is switched on and if the pixel is nontransparent. If both are favorable, the distance between the light position and the current pixel is compared with the light radius to determine if the pixel is inside the volume of illumination. Note that `gl_FragCoord.xyz` is the GLSL-defined variable for current pixel position and that this computation assumes pixel-space units. When all conditions are favorable, the color of the light is accumulated to the final results.
3. **Step C**: Apply the tint and set the final color via `gl_FragColor`.

```glsl
void main(void) {
    // Step A: sample the texture and apply ambient
    vec4 textureMapColor = texture2D(uSampler, vec2(vTexCoord.s, vTexCoord.t));
    vec4 lgtResults = uGlobalAmbientIntensity * uGlobalAmbientColor;
    
    // Step B: decide if the light should illuminate
    if (uLightOn && (textureMapColor.a > 0.0)) {
        float dist = length(uLightPosition.xyz - gl_FragCoord.xyz);
        if (dist <= uLightRadius)
            lgtResults += uLightColor;
    }
    lgtResults *= textureMapColor;
    
    // Step C: tint texture leave transparent area defined by texture
    vec3 r = vec3(lgtResults) * (1.0 - uPixelColor.a) + vec3(uPixelColor) * uPixelColor.a;
    vec4 result = vec4(r, textureMapColor.a);
    gl_FragColor = result;
}
```

## Defining a Light Class

With the GLSL `light_fs` shader defined, you can now define a class to encapsulate a point light source for the game engine:

1. Create a new `lights` folder in the `src/engine` folder. In the `lights` folder, add a new file and name it `lights.js`.
2. Edit `lights.js` to create the `Light` class, and define the constructor to initialize the light color, position, radius, and on/off status. Remember to export the class.

```javascript
class Light {
    constructor() {
        this.mColor = vec4.fromValues(0.1, 0.1, 0.1, 1); // light color
        this.mPosition = vec3.fromValues(0, 0, 5); // WC light position
        this.mRadius = 10; // effective radius in WC
        this.mIsOn = true;
    }
    // ... implementation to follow ...
}
export default Light;
```

3. Define the getters and setters for the instance variables:

```javascript
// simple setters and getters
setColor(c) { this.mColor = vec4.clone(c); }
getColor() { return this.mColor; }

set2DPosition(p) {
    this.mPosition = vec3.fromValues(p[0], p[1], this.mPosition[2]);
}
setXPos(x) { this.mPosition[0] = x; }
setYPos(y) { this.mPosition[1] = y; }
setZPos(z) { this.mPosition[2] = z; }
getPosition() { return this.mPosition; }

setRadius(r) { this.mRadius = r; }
getRadius() { return this.mRadius; }

setLightTo(isOn) { this.mIsOn = isOn; }
isLightOn() { return this.mIsOn; }
```

Lastly, remember to update the engine access file, `index.js`, to forward the newly defined functionality to the client.

## Defining the LightShader Class

The `LightShader` class subclasses from `SpriteShader` to encapsulate the communication of the values that are specific to the uniform variables defined for a point light source in the `light_fs` fragment shader. In this way, the `LightShader` class can serve as a convenient interface for the GLSL fragment shader.

1. In the `src/engine/shaders` folder, create a new file and name it `light_shader.js`.
2. Define the `LightShader` class to be a subclass of `SpriteShader`. In the constructor, define the necessary variables to support sending the information associated with a point light to the `light_fs` fragment shader. The point light information in the engine is stored in `mLight`, while the reference to the `Camera` is important to convert all information from WC to pixel space. The last four lines of the constructor query to obtain the reference locations to the uniform variables in `light_fs`. Don't forget to export the class.

```javascript
import SpriteShader from "./sprite_shader.js";
import * as glSys from "../core/gl.js";

class LightShader extends SpriteShader {
    constructor(vertexShaderPath, fragmentShaderPath) {
        // Call super class constructor
        super(vertexShaderPath, fragmentShaderPath);
        
        // glsl uniform position references
        this.mColorRef = null;
        this.mPosRef = null;
        this.mRadiusRef = null;
        this.mIsOnRef = null;
        this.mLight = null;  // the light source in the Game Engine
        this.mCamera = null; // camera to draw, need for WC to DC xform
        
        // create the references to these uniforms in the LightShader
        let shader = this.mCompiledShader;
        let gl = glSys.get();
        this.mColorRef = gl.getUniformLocation(shader, "uLightColor");
        this.mPosRef = gl.getUniformLocation(shader, "uLightPosition");
        this.mRadiusRef = gl.getUniformLocation(shader, "uLightRadius");
        this.mIsOnRef = gl.getUniformLocation(shader, "uLightOn");
    }
    // ... implementation to follow ...
}
export default LightShader;
```

3. Define a simple setter function to associate a light and camera with the shader:

```javascript
setCameraAndLight(c, l) {
    this.mCamera = c;
    this.mLight = l;
}
```

4. Override the `activate()` function to append the new functionality of loading the point light information in `mLight` when the light is present. Notice that you still call the `activate()` function of the super class to communicate the rest of the values to the uniform variables of the `light_fs` fragment shader.

```javascript
activate(pixelColor, trsMatrix, cameraMatrix) {
    // first call the super class' activate
    super.activate(pixelColor, trsMatrix, cameraMatrix);
    if (this.mLight !== null) {
        this._loadToShader();
    } else {
        glSys.get().uniform1i(this.mIsOnRef, false); // switch off light!
    }
}
```

5. Implement the `_loadToShader()` function to communicate the values of the point light to the uniform variables in the shader. Recall that this communication is performed via the references created in the constructor and the set uniform functions. It is important to note that the camera provides the new coordinate space transformation functionality of `wcPosToPixel()` and `wcSizeToPixel()`. These two functions ensure corresponding values in the `light_fs` are in pixel space such that relevant computations such as distances between positions can be performed. The implementation of these functions will be examined shortly.

```javascript
_loadToShader(aCamera) {
    let gl = glSys.get();
    gl.uniform1i(this.mIsOnRef, this.mLight.isLightOn());
    if (this.mLight.isLightOn()) {
        let p = this.mCamera.wcPosToPixel(this.mLight.getPosition());
        let r = this.mCamera.wcSizeToPixel(this.mLight.getRadius());
        let c = this.mLight.getColor();
        gl.uniform4fv(this.mColorRef, c);
        gl.uniform3fv(this.mPosRef, vec3.fromValues(p[0], p[1], p[2]));
        gl.uniform1f(this.mRadiusRef, r);
    }
}
```

## Defining the LightRenderable Class

With `LightShader` defined to interface to the GLSL `light_fs` shader, you can now focus on defining a new `Renderable` class for the game programmer. It is important that a light can shine on and illuminate all `Renderable` types, including those with texture and animated sprites. For this reason, the new class must encapsulate all existing `Renderable` functionality and be a subclass of `SpriteAnimateRenderable`. You can think of this new class as a `SpriteAnimateRenderable` that can be illuminated by a `Light` object.

1. 
