

## 以“_”开头命名的函数为私有或受保护函数

以“_”开头命名的函数为私有或受保护函数，不应在类外部调用。这是本书遵循的约定，而非 JavaScript 强制要求的。

## 定义 TextureRenderable 类

您现在可以定义 `TextureRenderable` 类了。如前所述，`TextureRenderable` 派生自 `Renderable` 类，并扩展了其功能以渲染纹理映射对象。

### 1. 创建类文件和构造函数

在 `src/engine/renderables` 文件夹中创建一个新文件，并将其命名为 `texture_renderable.js`。添加构造函数。回顾一下，`super()` 是对超类（`Renderable`）构造函数的调用；同样，`super.setColor()` 和 `super._setShader()` 是对超类函数的调用。正如在讨论引擎纹理资源模块时将详述的那样，`myTexture` 参数是包含纹理图像的文件路径。

```javascript
class TextureRenderable extends Renderable {
    constructor(myTexture) {
        super();
        super.setColor([1, 1, 1, 0]); // Alpha 0: 无纹理着色
        super._setShader(shaderResources.getTextureShader());
        this.mTexture = myTexture;  // 不能为 "null"
    }
    ... 后续实现 ...
}
```

### 2. 定义 `draw()` 函数

定义 `draw()` 函数以追加 `Renderable` 类中定义的功能来支持纹理。`texture.activate()` 函数激活特定纹理并允许使用该纹理进行绘制。该函数的细节将在下一节讨论。

```javascript
draw(camera) {
    // 激活纹理
    texture.activate(this.mTexture);
    super.draw(camera);
}
```

### 3. 定义纹理引用的 getter 和 setter

```javascript
getTexture() { return this.mTexture; }
setTexture(newTexture) { this.mTexture = newTexture; }
```

### 4. 导出类

```javascript
export default TextureRenderable;
```

## 引擎中的纹理支持

为了支持使用纹理进行绘制，游戏引擎的其余部分需要进行两项主要修改：WebGL 上下文配置和一个专用的引擎组件，用于支持与纹理相关的操作。

### 配置 WebGL 以支持纹理

必须更新 WebGL 上下文的配置以支持纹理。在 `gl.js` 中，根据以下内容更新 `init()` 函数：

```javascript
function init(htmlCanvasID) {
    mCanvas = document.getElementById(htmlCanvasID);
    if (mCanvas == null)
        throw new Error("Engine init [" + htmlCanvasID + "] HTML element id not found");
    // 标准或实验性的 webgl 并绑定到 Canvas 区域
    // 将结果存储到实例变量 mGL
    mGL = mCanvas.getContext("webgl2", {alpha: false}) ||
          mCanvas.getContext("experimental-webgl2", {alpha: false});
    if (mGL === null) {
        document.write("<br><b>WebGL 2 is not supported!</b>");
        return;
    }
    // 允许纹理透明度。
    mGL.blendFunc(mGL.SRC_ALPHA, mGL.ONE_MINUS_SRC_ALPHA);
    mGL.enable(mGL.BLEND);
    // 设置图像翻转 y 轴以匹配纹理坐标空间。
    mGL.pixelStorei(mGL.UNPACK_FLIP_Y_WEBGL, true);
}
```

传递给 `mCanvas.getContext()` 的参数通知浏览器 Canvas 应该是不透明的。这可以加快透明内容和图像的绘制速度。`blendFunc()` 函数在绘制带有 Alpha 通道的图像时启用透明度。`pixelStorei()` 函数将 uv 坐标的原点定义在左下角。

### 创建纹理资源模块

与文本和音频文件类似，必须定义一个新的引擎组件来支持相应的纹理操作，包括从服务器文件系统加载、通过 WebGL 上下文存储到 GPU 内存、激活纹理缓冲区以进行绘制，以及从 GPU 中移除。

#### 1. 创建纹理资源文件

在 `src/engine/resources` 文件夹中创建一个新文件，并将其命名为 `texture.js`。该文件将实现 `Texture` 引擎组件。

#### 2. 定义 `TextureInfo` 类

定义 `TextureInfo` 类以在游戏引擎中表示纹理。`mWidth` 和 `mHeight` 是纹理图像的像素分辨率，`mGLTexID` 是对 WebGL 纹理存储的引用。

```javascript
class TextureInfo {
    constructor(w, h, id) {
        this.mWidth = w;
        this.mHeight = h;
        this.mGLTexID = id;
    }
}
```

> **Note**
>
> 出于效率原因，许多图形硬件仅支持图像分辨率为 2 的幂的纹理，例如 2x4 (2¹×2²)、4x16 (2²×2⁴)、64x256 (2⁶×2⁸) 等。WebGL 也是这种情况。本书中的所有示例仅适用于分辨率为 2 的幂的纹理。

#### 3. 导入核心资源管理功能

从 `resource_map` 导入核心资源管理功能：

```javascript
import * as map from "../core/resource_map.js"; // 来自 resource_map 的函数
let has = map.has;
let get = map.get;
```

#### 4. 定义 `load()` 函数

定义一个函数，以 Promise 形式异步加载图像，并将该 Promise 推入 map 中的待处理 Promise 队列。不同于文本和音频资源，JavaScript Image API 支持直接的图像文件加载，因此此处不需要 `map.loadDecodeParse()`。一旦图像加载完成，它将连同其文件路径（作为名称）一起传递给 `processLoadedImage()` 函数。

```javascript
// 加载纹理以便绘制。
function load(textureName) {
    let image = new Image();
    let texturePromise = new Promise(
        function(resolve) {
            image.onload = resolve;
            image.src = textureName;
        }
    ).then(
        function resolve() {
            processLoadedImage(textureName, image);
        }
    );
    map.pushPromise(texturePromise);
    return texturePromise;
}
```

#### 5. 定义 `unload()` 函数

添加 `unload()` 函数以清理引擎并释放 WebGL 资源：

```javascript
// 移除引用以允许关联内存
// 可用于后续的垃圾回收
function unload(textureName) {
    let texInfo = get(textureName);
    if (map.unload(textureName)) {
        let gl = glSys.get();
        gl.deleteTexture(texInfo.mGLTexID);
    }
}
```

#### 6. 定义 `processLoadedImage()` 函数

现在定义 `processLoadedImage()` 函数以转换图像格式并将其存储到 WebGL 上下文中。`gl.createTexture()` 函数创建一个 WebGL 纹理缓冲区并返回一个唯一 ID。`texImage2D()` 函数将图像存储到 WebGL 纹理缓冲区中，`generateMipmap()` 为纹理计算多级渐远纹理。最后，实例化一个 `TextureInfo` 对象来引用 WebGL 纹理，并根据纹理图像文件的文件路径将其存储到 `resource_map` 中。

```javascript
function processLoadedImage(path, image) {
    let gl = glSys.get();
    // 生成对 webGL 上下文的纹理引用
    let textureID = gl.createTexture();
    // 将纹理引用与 webGL 中的当前纹理绑定
    gl.bindTexture(gl.TEXTURE_2D, textureID);
    // 将纹理加载到带有描述信息的纹理数据结构中。
    // 参数：
    //  1: 纹理加载目标的“绑定点”或 target。
    //  2: 细节层级。用于多级渐远纹理。0 为基本纹理层级。
    //  3: 内部格式。每个元素的组成。即像素。
    //  4: 纹素数据格式。必须与内部格式匹配。
    //  5: 纹素数据的数据类型。
    //  6: 纹理数据。
    gl.texImage2D(gl.TEXTURE_2D, 0,
                  gl.RGBA, gl.RGBA, gl.UNSIGNED_BYTE, image);
    // 为此纹理创建多级渐远纹理。
    gl.generateMipmap(gl.TEXTURE_2D);
    // 告诉 WebGL 完成对 mGL.TEXTURE_2D 目标的数据操作。
    gl.bindTexture(gl.TEXTURE_2D, null);
    let texInfo = new TextureInfo(image.naturalWidth,
                                  image.naturalHeight, textureID);
    map.set(path, texInfo);
}
```

> **Note**

