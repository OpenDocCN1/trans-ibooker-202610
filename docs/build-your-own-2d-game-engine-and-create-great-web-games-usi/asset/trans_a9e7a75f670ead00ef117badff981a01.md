
# 摄像机实现

## 步骤 B：计算摄像机矩阵

```javascript
// Step B: Compute the Camera Matrix
let center = this.getWCCenter();
// Step B1: after translation, scale to -1 to 1: 2x2 square at origin
mat4.scale(this.mCameraMatrix, mat4.create(),
    vec3.fromValues(2.0 / this.getWCWidth(),
        2.0 / this.getWCHeight(), 1.0));
// Step B2: first translate camera center to the origin
mat4.translate(this.mCameraMatrix, this.mCameraMatrix,
    vec3.fromValues(-center[0], -center[1], 0));
```

再次强调，这段代码与前一个示例中的 `MyGame` 构造函数相似。

### 定义一个函数以访问计算出的摄像机矩阵

```javascript
getCameraMatrix() {
    return this.mCameraMatrix;
}
```

### 导出摄像机类

```javascript
export default Camera;
```

## 修改 Renderable 以支持摄像机类

`Renderable` 类的 `draw()` 函数必须修改为接收新定义的 `Camera`，以便访问计算出的摄像机矩阵：

```javascript
draw(camera) {
    let gl = glSys.get();
    this.mShader.activate(this.mColor, this.mXform.getTRSMatrix(),
        camera.getCameraMatrix());
    gl.drawArrays(gl.TRIANGLE_STRIP, 0, 4);
}
```

## 修改引擎访问文件以导出摄像机

保持引擎访问文件 `index.js` 的更新很重要，以便新定义的 `Camera` 类可以被游戏开发者访问：

1. 编辑 `index.js`；从新定义的 `camera.js` 文件导入：

```javascript
// general utilities
import Camera from "./camera.js";
import Transform from "./transform.js";
import Renderable from "./renderable.js";
```

2. 导出 `Camera` 供客户端访问：

```javascript
export default {
    // Util classes
    Camera, Transform, Renderable,
    // functions
    init, clearCanvas
}
```

## 测试摄像机

摄像机类正确定义后，从 `my_game.js` 测试它非常简单：

1. 编辑 `my_game.js`；在步骤 A 初始化游戏引擎后，创建一个 `Camera` 对象实例，其设置定义了步骤 B 中前一个项目的 WC 和视口：

```javascript
class MyGame {
    constructor(htmlCanvasID) {
        // Step A: Initialize the game engine
        engine.init(htmlCanvasID);
        // Step B: Setup the camera
        this.mCamera = new engine.Camera(
            vec2.fromValues(20, 60),  // center of the WC
            20,                       // width of WC
            [20, 40, 600, 300]        // viewport: orgX, orgY, W, H
        );
        // ... implementation to follow ...
    }
}
```

2. 继续在步骤 C 和 D 中创建六个 `Renderable` 对象并清除画布：

```javascript
// Step C: Create the Renderable objects:
this.mBlueSq = new engine.Renderable();
this.mBlueSq.setColor([0.25, 0.25, 0.95, 1]);
this.mRedSq = new engine.Renderable();
this.mRedSq.setColor([1, 0.25, 0.25, 1]);
this.mTLSq = new engine.Renderable();
this.mTLSq.setColor([0.9, 0.1, 0.1, 1]);
this.mTRSq = new engine.Renderable();
this.mTRSq.setColor([0.1, 0.9, 0.1, 1]);
this.mBRSq = new engine.Renderable();
this.mBRSq.setColor([0.1, 0.1, 0.9, 1]);
this.mBLSq = new engine.Renderable();
this.mBLSq.setColor([0.1, 0.1, 0.1, 1]);
// Step D: Clear the canvas
engine.clearCanvas([0.9, 0.9, 0.9, 1]);  // Clear the canvas
```

3. 现在，调用 `Camera` 对象的 `setViewAndCameraMatrix()` 函数以在步骤 E 中配置 WebGL 视口并计算摄像机矩阵，并在步骤 F 和 G 中使用 `Camera` 对象绘制所有 `Renderable` 对象。

```javascript
// Step E: Starts the drawing by activating the camera
this.mCamera.setViewAndCameraMatrix();
// Step F: Draw the blue square
// Center Blue, slightly rotated square
this.mBlueSq.getXform().setPosition(20, 60);
this.mBlueSq.getXform().setRotationInRad(0.2);  // In Radians
this.mBlueSq.getXform().setSize(5, 5);
this.mBlueSq.draw(this.mCamera);
// Step G: Draw the center and the corner squares
// center red square
this.mRedSq.getXform().setPosition(20, 60);
this.mRedSq.getXform().setSize(2, 2);
this.mRedSq.draw(this.mCamera);
// top left
this.mTLSq.getXform().setPosition(10, 65);
this.mTLSq.draw(this.mCamera);
// top right
this.mTRSq.getXform().setPosition(30, 65);
this.mTRSq.draw(this.mCamera);
// bottom right
this.mBRSq.getXform().setPosition(30, 55);
this.mBRSq.draw(this.mCamera);
// bottom left
this.mBLSq.getXform().setPosition(10, 55);
this.mBLSq.draw(this.mCamera);
```

`mCamera` 对象被传递给 `Renderable` 对象的 `draw()` 函数，以便检索并使用 `Camera` 变换矩阵操作符来激活着色器。

## 总结

在本章中，你学习了如何创建一个支持绘制多个对象的系统。该系统由三部分组成：对象、每个对象的细节、以及在浏览器画布上显示对象。对象由 `Renderable` 封装，它使用 `Transform` 来捕获其细节——位置、大小和旋转。显示对象的细节由 `Camera` 定义，特定位置的对象可以显示在画布上的理想子区域中。

你还了解到，所有对象都是相对于世界空间（WC）绘制的，这是一个方便的坐标系。WC 是基于坐标变换为场景构图定义的。最后，`Camera` 变换用于选择 WC 的哪一部分实际显示在浏览器的画布上。这可以通过定义一个 `Camera` 可视区域并使用 WebGL 提供的视口功能来实现。

当你构建绘制系统时，游戏引擎源代码结构已持续重构为抽象和封装的组件。这样，源代码结构继续支持进一步扩展，包括将在下一章讨论的额外功能。

© The Author(s), under exclusive license to APress Media, LLC, part of Springer Nature 2022 K. Sung et al. *构建你自己的 2D 游戏引擎并创建伟大的 Web 游戏* [https://doi.org/10.1007/978-1-4842-7377-7_4](https://doi.org/10.1007/978-1-4842-7377-7_4)

# 4. 实现视频游戏的常见组件

**Kelvin Sung**<sup>(1)</sup>, **Jebediah Pavleas**<sup>(2)</sup>, **Matthew Munson**<sup>(3)</sup> 和 **Jason Pace**<sup>(4)</sup>

1. Bothell, WA, USA
2. Kenmore, WA, USA
3. Lake Forest Park, WA, USA
4. Portland, OR, USA

## 摘要

完成本章后，你将能够：

- 控制 `Renderable` 对象的位置、大小和旋转，以构建复杂的运动和动画
- 接收玩家的键盘输入以控制和制作 `Renderable` 对象动画
- 处理外部资源的异步加载和卸载
- 从场景文件定义、加载并执行一个简单的游戏关卡
- 通过加载新场景来切换游戏关卡
- 处理用于背景音乐和音频提示的声音片段

## 简介

在前几章中，构建了一个骨架游戏引擎以支持基本绘制操作。绘制是构建游戏引擎的第一步，因为它允许你在继续扩展游戏引擎功能的同时观察输出。在本章中，将探讨并添加两个重要机制——交互性和资源支持——到游戏引擎中。交互性使引擎能够接收和解释玩家输入，而资源支持是指处理外部文件（如 GLSL 着色器源代码文件、音频片段和图像）的功能。
