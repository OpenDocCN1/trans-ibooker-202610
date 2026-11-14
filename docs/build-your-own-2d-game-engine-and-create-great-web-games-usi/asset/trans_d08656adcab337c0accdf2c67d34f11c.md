
虽然绘图能力是游戏引擎最基本的功能之一，但绘图实现的细节通常会干扰游戏玩法编程。例如，在足球游戏中，创建、控制位置和绘制足球运动员非常重要。然而，暴露每个玩家实际定义方式的细节（由形成三角形的顶点集合定义）会迅速让游戏开发过程变得难以应付且复杂。因此，游戏引擎为绘图操作提供定义良好的抽象接口非常重要。

通过结构良好的源代码，可以通过对相应文件夹进行局部更改来实现新概念，从而逐步且系统地增加游戏引擎的复杂性。第一项任务是扩展引擎以支持绘图的封装，从而可以将绘图操作作为逻辑实体或可渲染对象进行操作。

> **注意**
>
> 在计算机图形学和视频游戏的语境中，*render*（渲染）一词是指改变与抽象表示相对应的像素颜色的过程。例如，在上一章中，你学习了如何渲染一个正方形。

## 可渲染对象项目

该项目引入 `Renderable` 类来封装绘图操作。在接下来的几个项目中，你将学习更多支持概念来完善 `Renderable` 类的实现，以便可以创建和操作多个实例。图 [3-1](#334805_2_En_3_Chapter.xhtml#Fig1) 显示了运行可渲染对象项目的输出。该项目的源代码定义在 `chapter3/3.1.renderable_objects` 文件夹中。

![图 3-1 ¶ 运行可渲染对象项目](images/334805_2_En_3_Chapter/334805_2_En_3_Fig1_HTML.jpg)

**图 3-1 ¶ 运行可渲染对象项目**

项目目标如下：

*   重组源代码结构，以预期功能的增加
*   支持游戏引擎内部资源共享
*   通过 `index.js` 文件为游戏开发者引入系统化的接口
*   首先抽象相关的绘图功能，开始构建封装绘图操作的类的过程
*   演示创建多个 `Renderable` 对象的能力

### 源代码结构重组

在向游戏引擎引入额外功能之前，认识到上一个项目中引擎源代码组织的一些不足非常重要。特别请注意以下几点：

1.  `core.js` 源代码文件包含 WebGL 接口、引擎初始化和绘图功能。这些应该模块化以支持预期的系统复杂性增加。
2.  应定义一个系统来支持游戏引擎内部资源的共享。例如，`SimpleShader` 负责从游戏引擎到由 `simple_vs.glsl` 和 `simple_fs.glsl` 源代码文件编译而成的 GLSL 着色器的接口。由于编译后的着色器只有一份副本，因此只需要一个 `SimpleShader` 对象实例。游戏引擎应通过允许方便地创建和共享该对象来促进这一点。
3.  正如你所经历的，JavaScript `export` 语句可以成为隐藏详细实现的绝佳工具。然而，在大型复杂系统（如你即将开发的游戏引擎）中，从众多文件中确定要导入哪些类或模块可能会令人困惑且难以应付。应提供一个易于使用且系统化的接口，使游戏开发者（游戏引擎的用户）能够免受这些细节的干扰。

在下一节中，将重组游戏引擎源代码以解决这些问题。

#### 定义特定于 WebGL 的模块

源代码重组的第一步是识别并隔离内部功能，这些功能不应被游戏引擎的客户端访问：

1.  在你的项目中，在 `src/engine` 文件夹下创建一个新文件夹并命名为 `core`。从现在起，该文件夹将包含所有游戏引擎内部功能，并且不会导出给游戏开发者。
2.  你可以将上一个项目中的 `vertex_buffer.js` 源代码文件剪切并粘贴到 `src/engine/core` 文件夹中。图元顶点的细节是游戏引擎内部的，不应被游戏引擎的客户端看到或访问。
3.  在 `src/engine/core` 文件夹中创建一个新的源代码文件，命名为 `gl.js`，并定义 WebGL 的初始化和访问方法：

```javascript
"use strict"
let mCanvas = null;
let mGL = null;
function get() {
    return mGL;
}
function init(htmlCanvasID) {
    mCanvas = document.getElementById(htmlCanvasID);
    if (mCanvas == null)
        throw new Error("Engine init [" + htmlCanvasID + "] HTML element id not found");
    // Get standard or experimental webgl and binds to the Canvas area
    // store the results to the instance variable mGL
    mGL = mCanvas.getContext("webgl2") ||
          mCanvas.getContext("experimental-webgl2");
    if (mGL === null) {
        document.write("<br><b>WebGL 2 is not supported!</b>");
        return;
    }
}
export {init, get}
```

注意 `init()` 函数与上一个项目 `core.js` 中的 `initWebGL()` 函数相同。与之前的 `core.js` 源代码文件不同，`gl.js` 文件仅包含特定于 WebGL 的功能。

#### 定义内部着色器资源共享系统

由于 GLSL 着色器仅从 `simple_vs.glsl` 和 `simple_fs.glsl` 源代码文件创建和编译一份副本，因此游戏引擎内仅需要一个 `SimpleShader` 对象副本即可与编译后的着色器进行接口交互。你现在将创建一个简单的资源共享系统，以支持未来添加不同类型的着色器。

在 `src/engine/core` 文件夹中创建一个新的源代码文件，命名为 `shader_resources.js`，并定义 `SimpleShader` 的创建和访问方法。

> **注意**
>
> 回顾上一章，`SimpleShader` 类定义在位于 `src/engine` 文件夹中的 `simple_shader.js` 文件中。请记得从上一个项目复制所有相关源代码文件。

```javascript
"use strict";
import SimpleShader from "../simple_shader.js";
// Simple Shader
let kSimpleVS = "src/glsl_shaders/simple_vs.glsl"; // to VertexShader
let kSimpleFS = "src/glsl_shaders/simple_fs.glsl"; // to FragmentShader
let mConstColorShader = null;
function createShaders() {
    mConstColorShader = new SimpleShader(kSimpleVS, kSimpleFS);
}
function init() {
    createShaders();
}
function getConstColorShader() {
    return mConstColorShader;
}
export {init, getConstColorShader}
```

> **注意**
>
> 引用常量值的变量名称以小写 "k" 开头，如 `kSimpleVS`。

由于 `shader_resources` 模块位于 `src/engine/core` 文件夹中，因此定义的着色器在内部共享，无法从游戏引擎的客户端访问。

#### 为游戏开发者定义访问文件

你将定义一个引擎访问文件 `index.js`，以实现游戏引擎的基本功能，其目的类似于 C++ 头文件、Java 中的 `import` 语句或 C# 中的 `using` 语句，即无需深入了解引擎源代码结构即可随时访问功能。也就是说，通过导入 `index.js`，客户端可以访问引擎的所有组件和功能来构建他们的游戏。

1.  
