

# 键盘输入实现

## Update 函数

```javascript
function update() {
    let i;
    for (i = 0; i < keys.LastKeyCode; i++) {
        mIsKeyClicked[i] = (!mKeyPreviousState[i]) && mIsKeyPressed[i];
        mKeyPreviousState[i] = mIsKeyPressed[i];
    }
}
```

## 公共查询函数

添加公共函数以查询当前键盘状态，从而支持客户端游戏开发者：

```javascript
// 游戏引擎程序员用于测试某键是否被按下的函数
function isKeyPressed(keyCode) {
    return mIsKeyPressed[keyCode];
}

function isKeyClicked(keyCode) {
    return mIsKeyClicked[keyCode];
}
```

## 导出公共函数和常量

最后，导出公共函数和按键常量：

```javascript
export {keys, init, update, isKeyClicked, isKeyPressed};
```

# 修改引擎以支持键盘输入

为了正确支持输入，在游戏循环开始前，引擎必须初始化 `mIsKeyPressed`、`mIsKeyClicked` 和 `mKeyPreviousState` 数组。为了正确捕获玩家操作，在游戏循环核心的游戏过程中，必须相应地更新这些数组。

## 输入状态初始化 (index.js)

通过导入 `input.js` 模块、将输入初始化添加到引擎的 `init()` 函数中、并将输入模块添加到导出列表中以允许客户端游戏开发者访问，来修改 `index.js`。

```javascript
import * as input from "./input.js";

function init(htmlCanvasID) {
    glSys.init(htmlCanvasID);
    vertexBuffer.init();
    shaderResources.init();
    input.init();
}

export default {
    // 输入支持
    input,
    // 工具类
    Camera, Transform, Renderable,
    // 函数
    init, clearCanvas
};
```

## 游戏循环集成 (loop.js)

为了准确捕获键盘状态变化，输入组件必须与游戏循环的核心集成。通过向 `loop.js` 添加以下行，将输入的 `update()` 函数包含在核心游戏循环中。请注意其余代码保持不变。

```javascript
import * as input from "../input.js";

function loopOnce() {
    if (mLoopRunning) {
        ... identical to previous code ...
        // 步骤 D：按适当的次数更新游戏。
        //         仅每 kMPF (1/60 秒) 更新一次
        //         如果延迟大于更新帧数，则更新直到追上。
        while ((mLagTime >= kMPF) && mLoopRunning) {
            input.update();
            mCurrentScene.update();
            mLagTime -= kMPF;
        }
    }
}
```

# 测试键盘输入

您可以通过修改 `MyGame` 类中的 `Renderable` 对象来测试输入功能。将 `MyGame` `update()` 函数中的代码替换为以下内容：

```javascript
update() {
    // 简单游戏：移动白色方块并脉动红色方块
    let whiteXform = this.mWhiteSq.getXform();
    let deltaX = 0.05;
    // 步骤 A：测试白色方块移动
    if (engine.input.isKeyPressed(engine.input.keys.Right)) {
        if (whiteXform.getXPos() > 30) { // 窗口的右边界
            whiteXform.setPosition(10, 60);
        }
        whiteXform.incXPosBy(deltaX);
    }
    // 步骤 B：测试白色方块旋转
    if (engine.input.isKeyClicked(engine.input.keys.Up)) {
        whiteXform.incRotationByDegree(1);
    }
    let redXform = this.mRedSq.getXform();
    // 步骤 C：测试红色方块脉动
    if (engine.input.isKeyPressed(engine.input.keys.Down)) {
        if (redXform.getWidth() > 5) {
            redXform.setSize(2, 2);
        }
        redXform.incSizeBy(0.05);
    }
}
```

在上述代码中，步骤 A 确保按住右箭头键会将白色方块向右移动。步骤 B 检测上箭头键的按下然后释放事件。检测到此类事件时，白色方块会旋转。请注意，按住上箭头键不会产生持续的按键事件，因此不会导致白色方块持续旋转。步骤 C 测试按住下箭头键以使红色方块脉动。

您可以运行项目并包含用于操纵方块的其他控制。例如，增加对 `WASD` 键的支持以控制红色方块的位置。请再次注意，通过增加/减少位置变化量，您实际上是在控制对象移动的速度。

> **注意**
>
> “`WASD` 键”一词用于指代流行游戏控制的按键绑定：键 `W` 向上移动，`A` 向左移动，`S` 向下移动，`D` 向右移动。

# 资源管理与异步加载

视频游戏通常利用大量的美术资产或资源，包括音频片段和图像。支持游戏所需的资源可能非常庞大。此外，保持资源与实际游戏之间的独立性非常重要，以便它们可以独立更新，例如，在不更改游戏本身的情况下更改背景音频。出于这些原因，游戏资源通常存储在系统硬盘或网络上的服务器上。由于存储在游戏外部，这些资源有时被称为*外部资源*或*资产*。

游戏开始后，必须显式加载外部资源。为了高效利用内存，游戏应根据必要性动态加载和卸载资源。然而，加载外部资源可能涉及输入/输出设备操作或网络数据包延迟，因此可能耗时较长并潜在地影响实时交互。出于这些原因，在游戏的任何时刻，只有一部分资源保留在内存中，加载操作会被策略性地执行以避免中断游戏。在大多数情况下，每个关卡所需的资源会在该关卡的游戏过程中保留在内存中。通过这种方法，外部资源加载可以发生在关卡转换期间，此时玩家期待新的游戏环境，更有可能容忍加载造成的轻微延迟。

一旦加载完成，资源必须随时可访问以支持交互。资源的高效且有效管理对任何游戏引擎都至关重要。请注意游戏引擎负责的资源管理与资源实际所有权之间的明确区别。例如，游戏引擎必须支持高效加载和播放游戏的背景音乐，而实际拥有并提供背景音乐音频文件的是游戏本身（或游戏引擎的客户端）。在实现外部资源管理支持时，重要的是要记住实际资源不是游戏引擎的一部分。

此时，您一直在构建的游戏引擎仅处理一种资源——GLSL 着色器文件。回想一下，`SimpleShader` 对象在其构造函数中加载并编译 `simple_vs.glsl` 和 `simple_fs.glsl` 文件。到目前为止，着色器文件加载是通过同步 `XMLHttpRequest.open()` 完成的。这种同步加载是低效资源管理的一个例子，因为在浏览器尝试打开和加载着色器文件时，无法进行任何其他操作。一种高效的替代方案是发出异步加载命令，并在文件打开和加载时允许其他操作继续进行。

本节构建一个基础设施，以支持异步加载和对已加载资源的高效访问。基于此基础设施，在接下来的几个项目中，游戏引擎将扩展为支持场景转换期间的批量资源加载。

## 资源映射与着色器加载器项目

