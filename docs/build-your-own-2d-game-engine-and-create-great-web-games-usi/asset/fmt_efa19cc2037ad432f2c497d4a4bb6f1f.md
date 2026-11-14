
# SpriteShader 和 SpriteRenderable 实现指南

## SpriteShader 类

### 1. 实现 `setTextureCoordinate()` 方法

```javascript
setTextureCoordinate(texCoord) {
    let gl = glSys.get();
    gl.bindBuffer(gl.ARRAY_BUFFER, this.mTexCoordBuffer);
    gl.bufferSubData(gl.ARRAY_BUFFER, 0, new Float32Array(texCoord));
}
```

**注意**：`texCoord` 参数是一个包含 8 个浮点数的数组，用于指定纹理坐标位置给 WebGL 上下文。该数组的格式和内容由 WebGL 接口定义为：右上角、左上角、右下角、左下角。在你的案例中，这些应该是精灵图元素的四个角。

### 2. 重写 `_getTexCoordBuffer()` 方法

使得当着色器激活时，返回局部分配的动态缓冲区，而不是全局静态缓冲区。注意 `activate()` 函数是从 `TextureShader` 继承的。

```javascript
_getTexCoordBuffer() {
    return this.mTexCoordBuffer;
}
```

### 3. 导出类

```javascript
export default SpriteShader;
```

## SpriteRenderable 类

类似于 `Renderable` 类（使用 `SimpleShader` 渲染）和 `TextureRenderable` 类（使用 `TextureShader` 渲染），需要定义对应的 `SpriteRenderable` 类来表示将使用 `SpriteShader` 渲染的对象。

### 1. 创建文件

在 `src/engine/renderables` 文件夹下创建新文件，命名为 `sprite_renderable.js`。

### 2. 定义类和构造函数

继承自 `TextureRenderable` 类。注意四个实例变量 `mElmLeft`、`mElmRight`、`mElmTop` 和 `mElmBottom` 共同标识纹理空间中的一个子区域。这些是精灵图元素的边界。

```javascript
class SpriteRenderable extends TextureRenderable {
    constructor(myTexture) {
        super(myTexture);
        super._setShader(shaderResources.getSpriteShader());
        // 精灵坐标
        this.mElmLeft = 0.0;   // 纹理坐标边界
        this.mElmRight = 1.0;  // 0-左，1-右
        this.mElmTop = 1.0;    // 1-上，0-下
        this.mElmBottom = 0.0; // 图像的
    }
    // ... 后续实现 ...
}
```

### 3. 定义枚举数据类型

用于标识 WebGL 纹理坐标规范数组中对应的偏移位置：

```javascript
// 纹理坐标数组是一个包含 8 个浮点数的数组，其中元素：
// [0] [1]: 右上角的 u/v 坐标
// [2] [3]: 左上角的 u/v 坐标
// [4] [5]: 右下角的 u/v 坐标
// [6] [7]: 左下角的 u/v 坐标
const eTexCoordArrayIndex = Object.freeze({
    eLeft: 2,
    eRight: 0,
    eTop: 1,
    eBottom: 5
});
```

**注意**：枚举数据类型的名称以 "e" 开头，如 `eTexCoordArrayIndex`。

### 4. 定义设置 UV 值的函数

支持在纹理坐标空间（归一化到 0-1 之间）和像素位置（转换为 UV 值）中指定精灵图元素区域：

```javascript
// 通过纹理坐标指定元素区域（0 到 1 之间）
setElementUVCoordinate(left, right, bottom, top) {
    this.mElmLeft = left;
    this.mElmRight = right;
    this.mElmBottom = bottom;
    this.mElmTop = top;
}

// 通过像素位置定义元素区域（0 到图像分辨率）
setElementPixelPositions(left, right, bottom, top) {
    let texInfo = texture.get(this.mTexture);
    // 整个图像的宽度、高度
    let imageW = texInfo.mWidth;
    let imageH = texInfo.mHeight;
    this.mElmLeft = left / imageW;
    this.mElmRight = right / imageW;
    this.mElmBottom = bottom / imageH;
    this.mElmTop = top / imageH;
}
```

**注意**：`setElementPixelPositions()` 函数在存储结果到对应实例变量前，会将像素转换为纹理坐标。

### 5. 构建纹理坐标规范数组

用于传递给 WebGL 上下文：

```javascript
getElementUVCoordinateArray() {
    return [
        this.mElmRight,  this.mElmTop,     // 右上角的 x,y
        this.mElmLeft,   this.mElmTop,     // 左上角
        this.mElmRight,  this.mElmBottom,  // 右下角
        this.mElmLeft,   this.mElmBottom   // 左下角
    ];
}
```

### 6. 重写 `draw()` 函数

在实际绘制前将特定的纹理坐标值加载到 WebGL 上下文：

```javascript
draw(camera) {
    // 设置当前纹理坐标
    // 激活纹理
    this.mShader.setTextureCoordinate(this.getElementUVCoordinateArray());
    super.draw(camera);
}
```

### 7. 导出类和枚举类型

```javascript
export default SpriteRenderable;
export { eTexCoordArrayIndex };
```

## 在 shader_resources 中实现共享

类似于 `SimpleShader` 和 `TextureShader`，`SpriteShader` 是一个可共享的资源，因此应添加到引擎的 `shaderResources` 中。

### 1. 导入并定义获取器

在 `engine/core/shader_resources.js` 文件中，导入 `SpriteShader`，添加存储变量，并定义相应的获取器函数：

```javascript
import SpriteShader from "../shaders/sprite_shader.js";
let mSpriteShader = null;
function getSpriteShader() {
    return mSpriteShader;
}
```

### 2. 修改 `createShaders()` 函数

创建 `SpriteShader` 实例：

```javascript
function createShaders() {
    mConstColorShader = new SimpleShader(kSimpleVS, kSimpleFS);
    mTextureShader = new TextureShader(kTextureVS, kTextureFS);
    mSpriteShader = new SpriteShader(kTextureVS, kTextureFS);
}
```

**注意**：`SpriteShader` 实际上封装了 `texture_vs.glsl` 和 `texture_fs.glsl` 文件中定义的现有 GLSL 着色器。从 WebGL 角度看，带纹理绘制的功能保持不变。`SpriteShader` 的唯一区别在于纹理坐标值现在是可编程的。

### 3. 更新 `cleanUp()` 函数

确保正确释放资源：

```javascript
function cleanUp() {
    mConstColorShader.cleanUp();
    mTextureShader.cleanUp();
    mSpriteShader.cleanUp();
    // ... 与之前代码相同 ...
}
```

### 4. 导出新功能

```javascript
export { init, cleanUp,
    getConstColorShader, getTextureShader, getSpriteShader };
```

## 导出新功能给客户端

将精灵元素功能集成到引擎的最后一步是修改引擎访问文件 `index.js`。编辑 `index.js` 并添加以下导入和导出语句，授予客户端访问 `SpriteRenderable` 和 `eTexCoordArrayIndex`（用于访问 WebGL 纹理坐标数组的枚举数据类型）的权限。

```javascript
// renderables
import Renderable from "./renderables/renderable.js";
import TextureRenderable from "./renderables/texture_renderable.js";
import SpriteRenderable from "./renderables/sprite_renderable.js";
import { eTexCoordArrayIndex } from "./renderables/sprite_renderable.js";
// ... 与之前代码相同 ...
export default {
    // ... 与之前代码相同 ...
    // Renderables
    Renderable, TextureRenderable, SpriteRenderable,
    // constants
    eTexCoordArrayIndex,
    // functions
    init, cleanUp, clearCanvas
};
```

## 测试 SpriteRenderable

精灵元素和纹理坐标有两个重要功能需要测试：作为对象正确提取、绘制和控制精灵图元素；以及在对象上改变和控制 UV 坐标。为正确测试新增功能，必须修改 `my_game.js` 文件。

### 1. 构建、加载、卸载和绘制

`MyGame` 的构建、加载、卸载和绘制与之前的示例类似，此处不再重复详情。请参考 `src/my_game` 文件夹中的源代码获取详细信息。

### 2. 修改 `init()` 函数

按如下方式修改 `init()` 函数。
