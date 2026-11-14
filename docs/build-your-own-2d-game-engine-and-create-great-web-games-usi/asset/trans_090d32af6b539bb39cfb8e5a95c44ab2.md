
# 场景对象：游戏引擎的客户端接口

此时，在您的游戏引擎中，正在发生以下情况：

*   `window.onload` 函数初始化游戏引擎并调用 `loop.start()` 函数，将 `MyGame` 作为参数传入。
*   `loop.start()` 函数通过 `resource_map` 等待所有异步加载操作完成，然后调用初始化 `MyGame` 并启动实际的游戏循环周期。

从这个讨论中，有趣的是要认识到，任何具有适当定义的公共方法的对象都可以替换 `MyGame` 对象。实际上，在任何时候，都可以调用 `loop.start()` 函数来启动新场景的加载。本节将通过引入用于连接游戏引擎及其客户端的 `Scene` 对象来扩展这一想法。

## 场景对象项目

该项目将 `Scene` 定义为一个抽象超类，用于与您的游戏引擎进行交互。从该项目开始，所有客户端代码都必须封装在抽象 `Scene` 类的子类中，游戏引擎将能够以连贯且定义良好的方式与这些类进行交互。您可以在图 [4-5](#334805_2_En_4_Chapter.xhtml#Fig5) 中看到该项目运行的示例。该项目的源代码定义在 `chapter4/4.5.scene_objects` 文件夹中。

![图 4-5 ¶ 运行同时包含两个场景的场景对象项目](images/334805_2_En_4_Chapter/334805_2_En_4_Fig5_HTML.jpg)

**图 4-5 ¶ 运行同时包含两个场景的场景对象项目**

该项目中有两个不同的关卡：`MyGame` 关卡在灰色背景上绘制了一个位于红色正方形上方的蓝色矩形，而 `BlueLevel` 关卡在深蓝色背景上绘制了一个位于旋转白色正方形上方的红色矩形。为简单起见，两个关卡的控制方式相同。

*   **左/右箭头键**：使前景矩形左右移动
*   **Q 键**：退出游戏

请注意，在每个关卡中，将前景矩形向左移动以触碰左边界将导致加载另一个关卡。`MyGame` 关卡将导致加载 `BlueLevel`，而 `BlueLevel` 将导致加载 `MyGame` 关卡。

项目目标如下：

*   定义抽象 `Scene` 类以连接游戏引擎
*   体验游戏引擎对场景转换的支持
*   创建特定于场景的加载和卸载支持

### 抽象 Scene 类

基于前一个项目的经验，用于封装游戏引擎接口的抽象 `Scene` 类至少必须定义以下函数：`init()`、`draw()`、`update()`、`load()` 和 `unload()`。此列表中缺少对关卡转换的支持，包括开始、进入下一关卡，以及如果需要的话停止游戏。

#### 1. 创建 Scene 文件和导入

在 `src/engine` 文件夹中创建一个新的 JavaScript 文件，将其命名为 `scene.js`，并从 `loop` 模块和引擎访问文件 `index.js` 导入。这两个模块都是必需的，因为 `Scene` 对象必须在游戏关卡开始和结束时启动和停止游戏循环，并且如果关卡决定终止游戏，则必须清理引擎。

```javascript
import * as loop from "./core/loop.js";
import engine from "./index.js";
```

> **注意**
>
> 在 `Scene` 开始之前，游戏循环绝不能运行。这是因为在正在运行的游戏循环从 `Scene` 调用 `update()` 函数之前，所需的资源必须被正确加载。同样，关卡的卸载只能在游戏循环停止运行后执行。

#### 2. 定义错误对象

定义 JavaScript `Error` 对象，以便在客户端误用时发出警告：

```javascript
const kAbstractClassError = new Error("Abstract Class");
const kAbstractMethodError = new Error("Abstract Method");
```

#### 3. 创建并导出 Scene 类

创建一个名为 `Scene` 的新类并将其导出：

```javascript
class Scene {
    // ... 后续实现 ...
}
export default Scene;
```

#### 4. 实现构造函数

实现构造函数以确保仅实例化 `Scene` 类的子类：

```javascript
constructor() {
    if (this.constructor === Scene) {
        throw kAbstractClassError;
    }
}
```

#### 5. 定义场景转换函数

定义场景转换函数：`start()`、`next()` 和 `stop()`。`start()` 函数是一个 `async` 函数，因为它负责启动游戏循环，而游戏循环又在等待所有异步加载完成。`next()` 和 `stop()` 函数都会停止游戏循环并调用 `unload()` 函数来卸载已加载的资源。区别在于，`next()` 函数预期会被子类重写并调用，在卸载当前场景后，子类可以继续前进到下一关卡。卸载后，`stop()` 函数假定游戏已终止，并继续清理游戏引擎。

```javascript
async start() {
    await loop.start(this);
}
next() {
    loop.stop();
    this.unload();
}
stop() {
    loop.stop();
    this.unload();
    engine.cleanUp();
}
```

#### 6. 定义派生接口函数

定义其余的派生接口函数。请注意，`Scene` 类是一个抽象类，因为所有接口函数都是空的。虽然子类可以选择仅实现接口函数的一个选定子集，但 `draw()` 和 `update()` 函数不是可选的，因为它们共同构成了关卡的核心核心。

```javascript
init() { /* 初始化关卡（由 loop.start() 调用） */ }
load() { /* 加载必要资源 */ }
unload() { /* 卸载所有资源 */ }
// draw/update 必须由子类重写
draw() { throw kAbstractMethodError; }
update() { throw kAbstractMethodError; }
```

这些函数共同呈现了一种与游戏引擎交互的协议。预期子类将重写这些函数以实现实际的游戏行为。

> **注意**
>
> JavaScript *不* 支持抽象类。该语言不会阻止游戏程序员实例化 `Scene` 对象；然而，创建的实例将完全无用，错误消息将为他们提供适当的警告。

### 修改游戏引擎以支持 Scene 类
