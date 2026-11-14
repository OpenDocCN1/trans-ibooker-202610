
类似于 `ShaderLightAt` 类，`ShaderMaterial` 类被定义用于将材质加载到 GLSL 片段着色器中。这是一个内部引擎操作。游戏程序员没有理由访问此类，因此，引擎访问文件 `index.js` 不应被修改以转发此类的定义。

## 修改 IllumShader 类

回顾 `IllumShader` 类是引擎与对应的 GLSL `illum_fs` 片段着色器的接口。现在必须修改 `IllumShader` 类以支持 `illum_fs` 中新定义的 Phong 光照功能。这可以通过修改 `IllumShader` 来定义一个 `ShaderMaterial` 对象，将 `Material` 对象的内容加载到 `illum_fs` 片段着色器中来实现。

### 1. 编辑 illum_shader.js

编辑 `src/engine/shaders` 中的 `illum_shader.js` 以导入 `ShaderMaterial`，并修改构造函数以定义新变量 `mMaterial` 和 `mCameraPos` 以支持 Phong 光照计算。然后定义变量 `mMaterialLoader` 和 `mCameraPosRef` 以保持引用并将相应内容加载到着色器中的统一变量。

```javascript
import ShaderMaterial from "./shader_material.js";

constructor(vertexShaderPath, fragmentShaderPath) {
    // Call super class constructor
    super(vertexShaderPath, fragmentShaderPath);
    // this is the material property of the Renderable
    this.mMaterial = null;
    this.mMaterialLoader = new ShaderMaterial(this.mCompiledShader);
    let gl = glSys.get();
    // Reference to the camera position
    this.mCameraPos = null;  // points to a vec3
    this.mCameraPosRef = gl.getUniformLocation(
        this.mCompiledShader, "uCameraPosition");
    // reference to the normal map sampler
    this.mNormalSamplerRef = gl.getUniformLocation(
        this.mCompiledShader, "uNormalSampler");
}
```

### 2. 修改 activate() 函数

修改 `activate()` 函数以将材质和相机位置加载到 `illum_fs` 片段着色器：

```javascript
activate(pixelColor, trsMatrix, cameraMatrix) {
    // first call the super class' activate
    super.activate(pixelColor, trsMatrix, cameraMatrix);
    let gl = glSys.get();
    gl.uniform1i(this.mNormalSamplerRef, 1); // binds to texture unit 1
    this.mMaterialLoader.loadToShader(this.mMaterial);
    gl.uniform3fv(this.mCameraPosRef, this.mCameraPos);
}
```

### 3. 定义 setMaterialAndCameraPos() 函数

定义 `setMaterialAndCameraPos()` 函数以设置 Phong 光照计算的相应变量：

```javascript
setMaterialAndCameraPos(m, p) {
    this.mMaterial = m;
    this.mCameraPos = p;
}
```

## 修改 IllumRenderable 类

现在可以修改 `IllumRenderable` 类以包含材质属性并正确支持 `IllumShader`。这是一个直接的更改。

### 1. 编辑 illum_renderable.js

编辑 `src/engine/renderables` 文件夹中的 `illum_renderable.js`，并修改构造函数以实例化一个新的 `Material` 对象：

```javascript
import Material from "../material.js";

constructor(myTexture, myNormalMap) {
    ... identical to previous code ...
    // Material for this Renderable
    this.mMaterial = new Material();
}
```

### 2. 更新 draw() 函数

更新 `draw()` 函数以在实际渲染之前将材质和相机位置设置到着色器。注意在调用 `camera.getWCCenterInPixelSpace()` 时，相机位置被正确转换到像素空间。

```javascript
draw(camera) {
    texture.activate(this.mNormalMap, glSys.get().TEXTURE1);
    this.mShader.setMaterialAndCameraPos(
        this.mMaterial, camera.getWCCenterInPixelSpace());
    super.draw(camera);
}
```

### 3. 定义材质对象的简单访问器

```javascript
getMaterial() { return this.mMaterial; }
```

## 修改 Camera 类

正如你在 `illum_fs` 片段着色器实现中所见，计算 ![$$ \\hat{V} $$](images/334805_2_En_8_Chapter/334805_2_En_8_Chapter_TeX_IEq43.png) 向量所需的相机位置必须在像素空间中。`Camera` 对象必须被修改以提供此类信息。由于 `Camera` 对象将其位置存储在世界坐标（WC）空间中，因此对于每个渲染的 `IllumRenderable` 对象，此位置必须被转换到像素空间。

场景中可能有大量 `IllumRenderable` 对象，且一旦渲染开始，相机位置就不能改变。这些观察表明，像素空间相机位置应在每个绘制周期中计算一次并缓存。`PerRenderCache` 类在“Simple Light Shader”项目中专门定义用于缓存每绘制周期信息，非常适合缓存像素空间相机位置。

### 1. 编辑 camera_main.js

编辑 `camera_main.js` 文件并在 `PerRenderCache` 中添加一个 `vec3` 以缓存相机在像素空间中的位置：

```javascript
class PerRenderCache {
    // Information to be updated once per render for efficiency concerns
    constructor() {
        this.mWCToPixelRatio = 1;  // WC to pixel transformation
        this.mCameraOrgX = 1; // Lower-left corner of camera in WC
        this.mCameraOrgY = 1;
        this.mCameraPosInPixelSpace = vec3.fromValues(0, 0, 0);
    }
}
```

### 2. 在 Camera 构造函数中定义 z 变量

在 `Camera` 构造函数中，定义一个 `z` 变量来模拟 `Camera` 对象与其余 `Renderable` 对象之间的距离。这第三个信息代表深度，是光照计算所需的。

```javascript
this.kCameraZ = 10; // this is for illumination computation
```

### 3. 在 setViewAndCameraMatrix() 的步骤 B4 中调用 wcPosToPixel()

在 `setViewAndCameraMatrix()` 函数的步骤 B4 中，调用 `wcPosToPixel()` 函数将相机位置转换到 3D 像素空间并缓存计算结果：

```javascript
// Step B4: compute and cache per-rendering information
this.mRenderCache.mWCToPixelRatio =
    this.mViewport[eViewport.eWidth] / this.getWCWidth();
this.mRenderCache.mCameraOrgX = center[0] - (this.getWCWidth() / 2);
this.mRenderCache.mCameraOrgY = center[1] - (this.getWCHeight() / 2);
let p = this.wcPosToPixel(this.getWCCenter());
this.mRenderCache.mCameraPosInPixelSpace[0] = p[0];
this.mRenderCache.mCameraPosInPixelSpace[1] = p[1];
this.mRenderCache.mCameraPosInPixelSpace[2] =
    this.fakeZInPixelSpace(this.kCameraZ);
```

### 4. 定义像素空间相机位置的访问器

```javascript
getWCCenterInPixelSpace() {
    return this.mRenderCache.mCameraPosInPixelSpace;
}
```

## 测试镜面反射

现在你可以测试 Phong 光照模型的实现，并观察改变对象的材质属性和镜面反射率的效果。由于背景、Hero 和左侧 Minion 已经是 `IllumRenderable` 对象的实例，这三个对象现在将表现出镜面反射。为了确保镜面反射的显著性，背景对象的镜面材质属性 `Ks` 在 `init()` 函数中被设置为亮红色。

一个新函数 `_selectCharacter()` 被定义以允许用户处理 Hero 或左侧 Minion 对象的材质属性。文件 `my_game_material_control.js` 实现了控制选定材质属性的实际用户交互。

## 观察

你可以运行项目并交互式地控制当前选定对象的材质属性（按键 5 选择左侧 Minion，按键 6 选择 Hero）。默认情况下，选择 Hero 对象的材质属性。你可以尝试通过按 E/R、T/Y 或 U/I 键来改变漫反射 RGB 分量。注意你可以同时按下多个键以同时改变多个颜色通道。
