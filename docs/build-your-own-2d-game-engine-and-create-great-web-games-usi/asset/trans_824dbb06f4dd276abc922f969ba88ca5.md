

## 修改 Renderable 以支持相机变换

```javascript
activate(pixelColor, trsMatrix, cameraMatrix) {
    let gl = glSys.get();
    gl.useProgram(this.mCompiledShader);
    // ... identical to previous code ...
    // load uniforms
    gl.uniform4fv(this.mPixelColorRef, pixelColor);
    gl.uniformMatrix4fv(this.mModelMatrixRef, false, trsMatrix);
    gl.uniformMatrix4fv(this.mCameraMatrixRef, false, cameraMatrix);
}
```

正如你之前所见，`gl.uniformMatrix4fv()` 函数将 `cameraMatrix` 的内容复制到 `uCameraXformMatrix` 操作符中。

回想一下，着色器是在 `Renderable` 类的 `draw()` 函数中激活的；因此，`Renderable` 也必须被修改以接收并传递 `cameraMatrix` 来激活着色器：

```javascript
draw(cameraMatrix) {
    let gl = glSys.get();
    this.mShader.activate(this.mColor,
        this.mXform.getTRSMatrix(),
        cameraMatrix);
    gl.drawArrays(gl.TRIANGLE_STRIP, 0, 4);
}
```

现在可以设置一个用于绘制的世界坐标系 (WC)，并在画布上定义一个子区域进行绘制。

## 设计场景

如图 [3-13](#334805_2_En_3_Chapter.xhtml#Fig13) 所示，出于测试目的，世界坐标系 (WC) 将被定义为以 (20, 60) 为中心，尺寸为 20×10。两个旋转的正方形，一个 5x5 的蓝色正方形和一个 2×2 的红色正方形，将被绘制在 WC 的中心。为了验证坐标边界，一个具有独特颜色的 1×1 正方形将被绘制在 WC 的每个角落。

![Figure 3-13 ¶ Designing a WC to support drawing](images/334805_2_En_3_Chapter/334805_2_En_3_Fig13_HTML.png)

**图 3-13 ¶ 设计用于支持绘制的 WC**

如图 [3-14](#334805_2_En_3_Chapter.xhtml#Fig14) 所示，WC 将被绘制到一个视口中，该视口的左下角位于 (20, 40)，尺寸为 600×300 像素。重要的是要注意，为了使正方形按比例显示，WC 的宽高比必须与视口的宽高比匹配。在这种情况下，WC 具有 20:10 的宽高比，这个 2:1 的比例与视口的 600:300 相匹配。

![Figure 3-14 ¶ Drawing the WC to the viewport](images/334805_2_En_3_Chapter/334805_2_En_3_Fig14_HTML.png)

**图 3-14 ¶ 将 WC 绘制到视口**

请注意，WC 的细节（以 (20, 60) 为中心，尺寸 20x10）和视口的细节（左下角位于 (20, 40)，尺寸 600x300）的选择是相当随意的。这些只是能够演示实现正确性的合理值。

### 实现设计

`MyGame` 类将被修改以实现该设计：

#### 1. 编辑 `my_game.js`

在构造函数中，执行步骤 A 初始化游戏引擎，以及步骤 B 创建六个 `Renderable` 对象（两个绘制在中心，四个绘制在 WC 的每个角落），并设置相应的颜色。

```javascript
constructor(htmlCanvasID) {
    // Step A: Initialize the game engine
    engine.init(htmlCanvasID);
    // Step B: Create the Renderable objects:
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
    // ... implementation to follow ...
}
```

#### 2. 步骤 C 和 D

步骤 C 和 D 清除整个画布，设置视口，并用不同的颜色清除视口：

```javascript
// Step C: Clear the entire canvas first
engine.clearCanvas([0.9, 0.9, 0.9, 1]);
// get access to the gl connection to the GPU
let gl = glSys.get();
// Step D: Setting up Viewport
// Step D1: Set up the viewport: area on canvas to be drawn
gl.viewport(
    20,     // x position of bottom-left corner of the area to be drawn
    40,     // y position of bottom-left corner of the area to be drawn
    600,    // width of the area to be drawn
    300     // height of the area to be drawn
);
// Step D2: set up the corresponding scissor area to limit clear area
gl.scissor(
    20,     // x position of bottom-left corner of the area to be drawn
    40,     // y position of bottom-left corner of the area to be drawn
    600,    // width of the area to be drawn
    300     // height of the area to be drawn
);
// Step D3: enable scissor area, clear and then disable the scissor area
gl.enable(gl.SCISSOR_TEST);
engine.clearCanvas([0.8, 0.8, 0.8, 1.0]);  // clear the scissor area
gl.disable(gl.SCISSOR_TEST);
```

步骤 D1 定义视口，步骤 D2 定义相应的裁剪区域。裁剪区域测试并限制要清除的区域。由于 `gl.scissor()` 涉及的测试在计算上开销较大，因此在使用后立即禁用它。

#### 3. 步骤 E：使用相机变换定义 WC

步骤 E 通过连接适当的缩放和平移操作符，使用相机变换定义 WC：

```javascript
// Step E: Set up camera transform matrix
// assume camera position and dimension
let cameraCenter = vec2.fromValues(20, 60);
let wcSize = vec2.fromValues(20, 10);
let cameraMatrix = mat4.create();
// Step E1: after translation, scale to: -1 to 1: a 2x2 square at origin
mat4.scale(cameraMatrix, mat4.create(),
    vec3.fromValues(2.0/wcSize[0], 2.0/wcSize[1], 1.0));
// Step E2: first to perform is to translate camera center to origin
mat4.translate(cameraMatrix, cameraMatrix,
    vec3.fromValues(-cameraCenter[0], -cameraCenter[1], 0));
```

步骤 E1 定义缩放操作符 `S(2/W, 2/H)`，将 WC 的 WxH 缩放到 NDC 的 2x2 尺寸，步骤 E2 定义平移操作符 `T(-center.x, -center.y)`，将 WC 与 NDC 中心对齐。请注意，连接顺序实现了先平移后缩放的操作符。这正是前面描述的相机变换，它定义 WC 如下：

- **中心**：(20,60)
- **左上角**：(10, 65)
- **右上角**：(30, 65)
- **右下角**：(30, 55)
- **左下角**：(10, 55)

回想一下，乘法顺序很重要，缩放和平移操作符的顺序不能互换。

#### 4. 步骤 F：绘制蓝色正方形

在 WC 中心设置略微旋转的 5x5 蓝色正方形，并使用相机变换操作符 `cameraMatrix` 进行绘制：

```javascript
// Step F: Draw the blue square
// Center Blue, slightly rotated square
this.mBlueSq.getXform().setPosition(20, 60);
this.mBlueSq.getXform().setRotationInRad(0.2); // In Radians
this.mBlueSq.getXform().setSize(5, 5);
this.mBlueSq.draw(cameraMatrix);
```

#### 5. 步骤 G：绘制中心和角落正方形

现在绘制其他五个正方形，首先是中心的 2x2 正方形，以及 WC 每个角落的一个正方形：

```javascript
// Step G: Draw the center and the corner squares
// center red square
this.mRedSq.getXform().setPosition(20, 60);
this.mRedSq.getXform().setSize(2, 2);
this.mRedSq.draw(cameraMatrix);
// top left
this.mTLSq.getXform().setPosition(10, 65);
this.mTLSq.draw(cameraMatrix);
// top right
this.mTRSq.getXform().setPosition(30, 65);
this.mTRSq.draw(cameraMatrix);
// bottom right
this.mBRSq.getXform().setPosition(30, 55);
this.mBRSq.draw(cameraMatrix);
// bottom left
this.mBLSq.getXform().setPosition(10, 55);
this.mBLSq.draw(cameraMatrix);
```

