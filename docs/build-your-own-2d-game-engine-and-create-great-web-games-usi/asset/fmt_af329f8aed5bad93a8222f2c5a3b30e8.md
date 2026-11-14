
## ShaderLightAt 类实现

```javascript
_setShaderReferences(aLightShader, index) {
    let gl = glSys.get();
    this.mColorRef = gl.getUniformLocation(
        aLightShader, "uLights[" + index + "].Color");
    this.mPosRef = gl.getUniformLocation(
        aLightShader, "uLights[" + index + "].Position");
    this.mNearRef = gl.getUniformLocation(
        aLightShader, "uLights[" + index + "].Near");
    this.mFarRef = gl.getUniformLocation(
        aLightShader, "uLights[" + index + "].Far");
    this.mIntensityRef = gl.getUniformLocation(
        aLightShader, "uLights[" + index + "].Intensity");
    this.mIsOnRef = gl.getUniformLocation(
        aLightShader, "uLights[" + index + "].IsOn");
}
```

### 步骤 3：实现 `loadToShader()` 函数

实现 `loadToShader()` 函数以将光源属性推送到 `light_fs` 片段着色器。注意此函数类似于之前项目中 `light_shader.js` 文件中定义的 `_loadToShader()` 函数。重要区别在于，在这种情况下，光源信息被加载到特定的数组索引。

```javascript
loadToShader(aCamera, aLight) {
    let gl = glSys.get();
    gl.uniform1i(this.mIsOnRef, aLight.isLightOn());
    if (aLight.isLightOn()) {
        let p = aCamera.wcPosToPixel(aLight.getPosition());
        let n = aCamera.wcSizeToPixel(aLight.getNear());
        let f = aCamera.wcSizeToPixel(aLight.getFar());
        let c = aLight.getColor();
        gl.uniform4fv(this.mColorRef, c);
        gl.uniform3fv(this.mPosRef, vec3.fromValues(p[0], p[1], p[2]));
        gl.uniform1f(this.mNearRef, n);
        gl.uniform1f(this.mFarRef, f);
        gl.uniform1f(this.mIntensityRef, aLight.getIntensity());
    }
}
```

### 步骤 4：定义 `switchOffLight()` 函数

定义一个简单函数以更新 `light_fs` 片段着色器数组中光源的开/关状态：

```javascript
switchOffLight() {
    let gl = glSys.get();
    gl.uniform1i(this.mIsOnRef, false);
}
```

注意 `ShaderLightAt` 类是为将光源加载到 GLSL 片段着色器中的特定数组元素而定义的。这是内部引擎操作。游戏程序员无需访问此类，因此引擎访问文件 `index.js` 不应修改以转发此类的定义。

## 修改 LightShader 类

现在必须修改 `LightShader` 对象以正确处理 `Light` 对象与 `light_fs` 片段着色器中光源数组之间的通信：

### 步骤 1：编辑 `light_shader.js`

首先编辑 `light_shader.js` 文件，导入 `ShaderLightAt`，并**移除** `_loadToShader()` 函数。实际的光源信息加载到 `light_fs` 片段着色器现在由新定义的 `ShaderLightAt` 对象处理。

```javascript
import ShaderLightAt from "./shader_light_at.js";
```

### 步骤 2：修改构造函数

修改构造函数以定义 `mLights`，即 `ShaderLightAt` 对象数组，对应 `light_fs` 片段着色器中定义的 `uLights` 数组。重要的是要注意 `mLights` 和 `uLights` 数组必须完全相同大小。

```javascript
constructor(vertexShaderPath, fragmentShaderPath) {
    // Call super class constructor
    super(vertexShaderPath, fragmentShaderPath);
    this.mLights = null;  // lights from the Renderable
    this.mCamera = null;  // camera to draw, need for WC to DC xform
    // *****WARNING**********
    // MUST correspond to GLSL uLight[] array size (for LightFS.glsl)
    // **********************
    this.kGLSLuLightArraySize = 4;  // must be the same as LightFS.glsl
    this.mShaderLights = [];
    let i, ls;
    for (i = 0; i < this.kGLSLuLightArraySize; i++) {
        ls = new ShaderLightAt(this.mCompiledShader, i);
        this.mShaderLights.push(ls);
    }
}
```

### 步骤 3：修改 `activate()` 函数

修改 `activate()` 函数以迭代并通过调用相应的 `loadToShader()` 函数将每个 `ShaderLightAt` 对象的内容加载到 `light_fs` 着色器。回想一下 GLSL 片段着色器要求 for 循环控制变量为常量。这意味着 `uLights` 数组的所有元素将在每次 `light_fs` 调用时被处理。因此，确保所有未使用的光源被关闭非常重要。这由以下代码中的最后一个 while 循环确保：

```javascript
activate(pixelColor, trsMatrix, cameraMatrix) {
    // first call the super class' activate
    super.activate(pixelColor, trsMatrix, cameraMatrix);
    // now push the light information to the shader
    let numLight = 0;
    if (this.mLights !== null) {
        while (numLight < this.mLights.length) {
            this.mShaderLights[numLight].loadToShader(
                this.mCamera, this.mLights[numLight]);
            numLight++;
        }
    }
    // switch off the left over ones.
    while (numLight < this.kGLSLuLightArraySize) {
        this.mShaderLights[numLight].switchOffLight(); // off the un-use
        numLight++;
    }
}
```

### 步骤 4：重命名 `setCameraAndLight()` 为 `setCameraAndLights()`

重命名 `setCameraAndLight()` 函数为 `setCameraAndLights()`；除了设置相应变量外，检查以确保光源数组大小不大于 `light_fs` 片段着色器中定义的数组大小。最后，记得在 `sprite_shader.js` 中更新相应的函数名。

```javascript
setCameraAndLights(c, l) {
    this.mCamera = c;
    this.mLights = l;
    if (this.mLights.length > this.kGLSLuLightArraySize)
        throw new Error ("Error: " ...);
}
```

## 修改 LightRenderable 类

现在可以修改 `LightRenderable` 类以支持多光源：

### 步骤 1：更新构造函数

在 `LightRenderable` 构造函数中，将单个光源引用变量替换为数组：

```javascript
constructor(myTexture) {
    super(myTexture);
    super._setShader(defaultShaders.getLightShader());
    // the light sources
    this.mLights = [];
}
```

### 步骤 2：更新 `draw()` 函数

确保更新 `draw` 函数以反映多光源的变化：

```javascript
draw(camera) {
    this.mShader.setCameraAndLights(camera, this.mLights);
    super.draw(camera);
}
```

### 步骤 3：定义光源数组的访问器函数

```javascript
getLightAt(index) { return this.mLights[index]; }
addLight(l) { this.mLights.push(l); }
```

## 使用 MyGame 测试光源

通过引擎中多光源支持的正确集成，现在可以修改 `MyGame` 来测试实现并检查结果。除了向场景添加多个光源外，还将添加控制每个光源属性的能力。为了保持可读性，将光源实例化和控制分离到不同的文件中。为了避免冗余和重复的代码列表，不展示简单实现的细节。

### 步骤 1：修改 `my_game_main.js`

修改 `src/my_game` 文件夹中的 `my_game_main.js` 文件以反映构造函数、`initialize` 函数、`draw` 函数和 `update` 函数的变化。所有这些变化都围绕通过光源集合处理多光源。

### 步骤 2：创建 `my_game_lights.js`

在 `src/my_game` 文件夹中，创建新文件 `my_game_lights.js` 以从 `my_game_main.js` 导入 `MyGame` 类并添加实例化和初始化光源的功能。

### 步骤 3：创建 `my_game_light_control.js`

在 `src/my_game` 文件夹中，创建新文件 `my_game_light_control.js` 以从 `my_game_lights.js` 导入并继续向 `MyGame` 添加光源控制。

### 步骤 4：修改 `my_game.js`

修改 `my_game.js` 以从 `my_game_light_control.js` 导入，确保访问所有新定义的功能。

## 观察
