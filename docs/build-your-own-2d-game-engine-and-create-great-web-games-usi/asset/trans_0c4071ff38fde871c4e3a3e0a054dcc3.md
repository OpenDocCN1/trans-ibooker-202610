
通过这个简单的修改，单位正方形的顶点位置将由 `uModelXformMatrix` 算子操作，因此正方形可以绘制到不同的位置。现在的任务是设置 `SimpleShader` 以将适当的变换算子加载到 `uModelXformMatrix` 中。

## 修改 SimpleShader 以加载变换算子

请遵循以下步骤：

1. 编辑 `simple_shader.js` 并添加一个实例变量以保存对顶点着色器中 `uModelXformMatrix` 矩阵的引用：

```
this.mModelMatrixRef = null;
```

2. 在 `SimpleShader` 构造函数的末尾，在步骤 E 下，设置对 `uPixelColor` 的引用后，添加以下代码以初始化此引用：

```
// Step E: Gets a reference to uniform variables in fragment shader
this.mPixelColorRef = gl.getUniformLocation(this.mCompiledShader, "uPixelColor");
this.mModelMatrixRef = gl.getUniformLocation(this.mCompiledShader, "uModelXformMatrix");
```

3. 修改 `activate()` 函数以接收第二个参数，并通过 `mModelMatrixRef` 将值加载到 `uModelXformMatrix`：

```
activate(pixelColor, trsMatrix) {
    let gl = glSys.get();
    gl.useProgram(this.mCompiledShader);
    ... identical to previous code ...
    // load uniforms
    gl.uniform4fv(this.mPixelColorRef, pixelColor);
    gl.uniformMatrix4fv(this.mModelMatrixRef, false, trsMatrix);
}
```

`gl.uniformMatrix4fv()` 函数将 `trsMatrix` 中的值复制到由 `this.mModelMatrixRef` 或顶点着色器中的 `uModelXformMatrix` 算子标识的顶点着色器位置。变量名 `trsMatrix` 表示它应该是一个包含平移 (T)、旋转 (R) 和缩放 (S) 连接结果的矩阵算子，即 TRS。

## 修改 Renderable 类以设置变换算子

编辑 `renderable.js` 以修改 `draw()` 函数，使其接收并转发一个变换算子给 `mShader.activate()` 函数，以便加载到 GLSL 着色器中：

```
draw(trsMatrix) {
    let gl = glSys.get();
    this.mShader.activate(this.mColor, trsMatrix);
    gl.drawArrays(gl.TRIANGLE_STRIP, 0, 4);
}
```

这样，当单位正方形的顶点被顶点着色器处理时，`uModelXformMatrix` 将包含用于变换顶点的适当算子，从而在所需的位置、大小和旋转角度绘制正方形。

## 测试变换

现在游戏引擎支持变换，你需要修改客户端代码以使用它进行绘制：

1. 编辑 `my_game.js`；在步骤 C 之后，不再激活并绘制两个正方形，而是替换步骤 C1 和 C2 以创建一个新的单位变换算子 `trsMatrix`：

```
// create a new identity transform operator
let trsMatrix = mat4.create();
```

2. 计算矩阵的连接，形成一个实现平移 (T)、旋转 (R) 和缩放 (S) 或 TRS 的单一变换算子：

```
// Step D: compute the white square transform
mat4.translate(trsMatrix, trsMatrix, vec3.fromValues(-0.25, 0.25, 0.0));
mat4.rotateZ(trsMatrix, trsMatrix, 0.2);        // rotation is in radian
mat4.scale(trsMatrix, trsMatrix, vec3.fromValues(1.2, 1.2, 1.0));
// Step E: draw the white square with the computed transform
this.mWhiteSq.draw(trsMatrix);
```

步骤 D 连接了 T(-0.25, 0.25)，向左和向上移动；R(0.2)，顺时针旋转 0.2 弧度；以及 S(1.2, 1.2)，将大小增加 1.2 倍。连接顺序先应用缩放算子，然后是旋转，最后是平移，即 `trsMatrix = TRS`。在步骤 E 中，`Renderable` 对象使用 `trsMatrix` 算子绘制，即一个 1.2×1.2 的白色矩形，略微旋转并位于中心左上方。

3. 最后，步骤 F 定义 `trsMatrix` 算子以绘制一个 0.4×0.4 的正方形，旋转 45 度并位于画布中心右下方，步骤 G 绘制红色正方形：

```
// Step F: compute the red square transform
mat4.identity(trsMatrix); // restart
mat4.translate(trsMatrix, trsMatrix, vec3.fromValues(0.25, -0.25, 0.0));
mat4.rotateZ(trsMatrix, trsMatrix, -0.785);   // about -45-degrees
mat4.scale(trsMatrix, trsMatrix, vec3.fromValues(0.4, 0.4, 1.0));
// Step G: draw the red square with the computed transform
this.mRedSq.draw(trsMatrix);
```

## 观察

运行项目，你应该能在画布上看到相应的白色和红色矩形。你可以通过更改数值来获得对算子的一些直观理解；例如，将正方形移动和缩放到不同位置和大小。你可以尝试通过移动相应的代码行来更改连接顺序；例如，将 `mat4.scale()` 移到 `mat4.translate()` 之前。你会注意到，通常变换结果不符合你的直觉。在本书中，你将始终按固定的 TRS 顺序应用变换算子。这种变换算子的顺序符合典型的人类直觉。大多数（如果不是全部）支持变换操作的图形 API 和应用程序都遵循 TRS 操作顺序。

现在你已经了解如何使用矩阵变换算子，是时候对它们进行抽象并隐藏其细节了。

# 封装变换算子

在之前的项目中，变换算子是直接基于矩阵计算的。虽然结果很重要，但计算过程涉及分散注意力的细节和重复代码。本项目将指导你遵循良好的编码实践，通过使用类隐藏详细计算来封装变换算子。这样，你可以在支持进一步扩展的同时保持可编程性，从而维护游戏引擎的模块化和易用性。

## Transform 对象项目

本项目定义了 `Transform` 类，以提供一个逻辑接口来操作矩阵变换算子并隐藏其细节。图 [3-7](#334805_2_En_3_Chapter.xhtml#Fig7) 显示了运行矩阵变换项目的输出。请注意，该项目的输出与前一个项目相同。该项目的源代码定义在 `chapter3/3.3.transform_objects` 文件夹中。

![图 3-7 ¶ 运行 Transform 对象项目](images/334805_2_En_3_Chapter/334805_2_En_3_Fig7_HTML.jpg)

**图 3-7 ¶ 运行 Transform 对象项目**

项目目标如下：

*   创建 `Transform` 类以封装矩阵变换功能
*   将 `Transform` 类集成到游戏引擎中
*   演示如何使用 `Transform` 对象

## Transform 类

继续基于前一个项目工作：

1. 在游戏引擎中定义 `Transform` 类，方法是在 `src/engine` 文件夹中创建一个新的源代码文件，并将其命名为 `transform.js`。

2. 定义构造函数以初始化对应算子的实例变量：`mPosition` 用于平移，`mScale` 用于缩放，`mRotationInRad` 用于旋转。

```
class Transform {
    constructor() {
        this.mPosition = vec2.fromValues(0, 0); // translation
        this.mScale = vec2.fromValues(1, 1);    // width (x), height (y)
        this.mRotationInRad = 0.0;              // in radians!
    }
    ... implementation to follow ...
}
```

3. 为每个算子的值添加 getter 和 setter：
