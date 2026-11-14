
# 编辑 gl.js 以启用 WebGL 模板缓冲区

编辑 `src/engine/core/gl.js` 文件，在引擎初始化期间启用并配置 WebGL 模板缓冲区。在 `init()` 函数中，添加在 WebGL 初始化期间请求分配和配置模板缓冲区及深度缓冲区的代码。注意，深度缓冲区（或 Z 缓冲区）也会被分配和配置。这对于正确的阴影投射器支持是必要的，因为阴影投射器必须位于接收器前方，或具有更大的 Z 深度，才能在接收器上投射阴影。

```javascript
function init(htmlCanvasID) {
    // ... identical to previous code ...
    mGL = mCanvas.getContext("webgl2",
        {alpha: false, depth: true, stencil: true}) ||
        mCanvas.getContext("experimental-webgl2",
            {alpha: false, depth: true, stencil: true});
    // ... identical to previous code ...
    // make sure depth testing is enabled
    mGL.enable(mGL.DEPTH_TEST);
    mGL.depthFunc(mGL.LEQUAL);
}
```

# 继续完善 gl.js：定义模板缓冲区绘制函数

继续处理 `gl.js`；定义开始、结束和禁用模板缓冲区绘制的函数。记得导出这些新的模板缓冲区支持函数。

```javascript
function beginDrawToStencil(bit, mask) {
    mGL.clear(mGL.STENCIL_BUFFER_BIT);
    mGL.enable(mGL.STENCIL_TEST);
    mGL.colorMask(false, false, false, false);
    mGL.depthMask(false);
    mGL.stencilFunc(mGL.NEVER, bit, mask);
    mGL.stencilOp(mGL.REPLACE, mGL.KEEP, mGL.KEEP);
    mGL.stencilMask(mask);
}

function endDrawToStencil(bit, mask) {
    mGL.depthMask(mGL.TRUE);
    mGL.stencilOp(mGL.KEEP, mGL.KEEP, mGL.KEEP);
    mGL.stencilFunc(mGL.EQUAL, bit, mask);
    mGL.colorMask(true, true, true, true);
}

function disableDrawToStencil() {
    mGL.disable(mGL.STENCIL_TEST);
}
```

# 编辑引擎访问文件 index.js

编辑 `src/engine/index.js` 中的 `clearCanvas()` 函数，在清空画布时同时清空模板缓冲区和深度缓冲区：

```javascript
function clearCanvas(color) {
    // ... identical to previous code ...
    gl.clear(gl.COLOR_BUFFER_BIT | gl.STENCIL_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
}
```

# 为游戏开发者定义阴影支持

正如定义 `ShadowCasterShader` 时所述，`Renderable` 类不应被定义为与阴影投射器和接收器着色器配对，因为这会赋予游戏开发者操纵算法创建对象作为常规游戏对象的能力。相反，引入 `ShadowCaster` 和 `ShadowReceiver` 类，允许游戏开发者创建阴影，而无需授予操纵底层几何体的权限。

## 定义阴影投射器类

不同于熟悉的 `Renderable` 类层次结构，`ShadowCaster` 类被定义为封装隐式定义的阴影投射器几何体的功能。回顾图 [8-25](#334805_2_En_8_Chapter.xhtml#Fig25)，阴影投射器几何体是基于阴影投射器（一个 `Renderable`）和阴影接收器（另一个 `Renderable`）对象的位置，为每个投射阴影的光源算法推导得出的。

为了支持在动画精灵元素上接收阴影，阴影接收器必须是 `SpriteRenderable` 或其子类。投射阴影的 `Renderable` 对象必须能够接收光源，因此它必须是 `LightRenderable` 或其子类。`ShadowCaster` 对象维护对实际投射和接收阴影的 `Renderable` 对象的引用，并定义为 `caster LightRenderable` 对象引用的每个光源计算和渲染阴影投射器几何体的算法。`ShadowCaster` 类的详细内容如下：

### 1. 创建阴影相关文件夹和文件

创建 `src/engine/shadows` 文件夹用于组织阴影相关支持文件，并在该文件夹中创建 `shadow_caster.js` 文件。

### 2. 定义 ShadowCaster 类和构造函数

定义 `ShadowCaster` 类和构造函数，初始化投射器几何体计算所需的实例变量和常量：

```javascript
import * as shaderResources from "../core/shader_resources.js";
import SpriteRenderable from "../renderables/sprite_renderable.js";
import Transform from "../utils/transform.js";
import { eLightType } from "../lights/light.js";

// shadowCaster: GameObject referencing at least a LightRenderable
// shadowReceiver: GameObject referencing at least a SpriteRenderable
class ShadowCaster {
    constructor(shadowCaster, shadowReceiver) {
        this.mShadowCaster = shadowCaster;
        this.mShadowReceiver = shadowReceiver;
        this.mCasterShader = shaderResources.getShadowCasterShader();
        this.mShadowColor = [0, 0, 0, 0.2];
        this.mSaveXform = new Transform();
        this.kCasterMaxScale = 3;      // Max amount a caster will be scaled
        this.kVerySmall = 0.001;
        this.kDistanceFudge = 0.01;    // to avoid caster-receiver overlap
        this.kReceiverDistanceFudge = 0.6; // Factor to reduce the projected caster geometry size
    }

    setShadowColor(c) {
        this.mShadowColor = c;
    }

    // ... implementation to follow ...
}

export default ShadowCaster;
```

`mShadowCaster` 是对至少包含 `LightRenderable` 的阴影投射器 `GameObject` 的引用，`mShadowReceiver` 是至少包含 `SpriteRenderable` 渲染组件的 `GameObject` 的引用。正如下一步详述的，`mCasterShader`、`mShadowColor` 和 `mSaveXform` 是支持渲染阴影投射器几何体的变量。

### 3. 实现 draw() 函数

实现 `draw()` 函数，为照亮 `mShadowCaster` 的 `Renderable` 对象的每个光源计算并绘制阴影投射器几何体：

```javascript
draw(aCamera) {
    let casterRenderable = this.mShadowCaster.getRenderable();
    // Step A: save caster xform/shader/color. Set caster to shadow color
    this.mShadowCaster.getXform().cloneTo(this.mSaveXform);
    let s = casterRenderable.swapShader(this.mCasterShader);
    let c = casterRenderable.getColor();
    casterRenderable.setColor(this.mShadowColor);
    let l, lgt;
    // Step B: loop through each light, if shadow casting is on
    //         compute the proper shadow offset
    for (l = 0; l < casterRenderable.getNumLights(); l++) {
        lgt = casterRenderable.getLightAt(l);
        if (lgt.isLightOn() && lgt.isLightCastShadow()) {
            // Step C: turn caster into caster geometry
            //         draws as SpriteRenderable
            this.mSaveXform.cloneTo(this.mShadowCaster.getXform());
            if (this._computeShadowGeometry(lgt)) {
                this.mCasterShader.setCameraAndLights(aCamera, lgt);
                SpriteRenderable.prototype.draw.call(
                    casterRenderable, aCamera);
            }
        }
    }
    // Step D: restore the original shadow caster
    this.mSaveXform.cloneTo(this.mShadowCaster.getXform());
    casterRenderable.swapShader(s);
    casterRenderable.setColor(c);
}
```

`casterRenderable` 是实际投射阴影的 `Renderable` 对象。`draw()` 函数包含以下四个主要步骤：

#### 步骤 A
保存投射器 `Renderable` 的状态、变换、着色器和颜色，并通过将其着色器设置为 `ShadowCasterShader`（`mCasterShader`）并将其颜色设置为阴影颜色，将其设为阴影投射器几何体。

#### 步骤 B
遍历照亮 `casterRenderable` 的所有光源，查找已开启且投射阴影的光源。

#### 步骤 C
对于每个产生阴影的光源，调用 `_computeShadowGeometry()` 函数计算适当大小和位置的阴影投射器几何体，并将其作为 `SpriteRenderable` 渲染。通过替换的 `ShadowCasterShader` 和阴影颜色，渲染的几何体呈现为实际 `casterRenderable` 的阴影。

#### 步骤 D
恢复 `casterRenderable` 的状态。

### 4. 后续实现
