

## 初始化输入组件

为了接收鼠标输入信息，输入组件需要访问 HTML 画布。编辑 `index.js` 并修改 `init()` 函数，以便在初始化期间将 `htmlCamvasID` 传递给输入组件。

```javascript
// 通用引擎工具
function init(htmlCanvasID) {
    glSys.init(htmlCanvasID);
    vertexBuffer.init();
    input.init(htmlCanvasID);
    audio.init();
    shaderResources.init();
    defaultResources.init();
}
```

## 在 input.js 中实现鼠标支持

与键盘输入类似，您应该通过编辑 `input.js` 向输入模块添加鼠标支持：

1. **定义鼠标按钮常量**

   编辑 `input.js` 并定义常量来表示三个鼠标按钮：

   ```javascript
   // 鼠标按钮枚举
   const eMouseButton = Object.freeze({
       eLeft: 0,
       eMiddle: 1,
       eRight: 2
   });
   ```

2. **定义鼠标输入变量**

   定义变量以支持鼠标输入。与键盘输入类似，鼠标按钮状态是包含三个布尔元素的数组，每个元素代表三个鼠标按钮的状态。

   ```javascript
   let mCanvas = null;
   let mButtonPreviousState = [];
   let mIsButtonPressed = [];
   let mIsButtonClicked = [];
   let mMousePosX = -1;
   let mMousePosY = -1;
   ```

3. **定义鼠标移动事件处理程序**

   定义鼠标移动事件处理程序：

   ```javascript
   function onMouseMove(event) {
       let inside = false;
       let bBox = mCanvas.getBoundingClientRect();
       // 现已处于画布空间。通过画布到客户端的比率进行转换。
       let x = Math.round((event.clientX - bBox.left) *
           (mCanvas.width / bBox.width));
       let y = Math.round((event.clientY - bBox.top) *
           (mCanvas.height / bBox.height));
       if ((x >= 0) && (x < mCanvas.width) &&
           (y >= 0) && (y < mCanvas.height)) {
           mMousePosX = x;
           mMousePosY = mCanvas.height - 1 - y;
           inside = true;
       }
       return inside;
   }
   ```

   请注意，鼠标事件处理程序首先检查位置是否在画布范围内，然后翻转 y 位置，使得位移相对于左下角进行测量，从而将原始像素位置转换为画布坐标空间。

4. **定义鼠标按下处理程序**

   定义鼠标按钮点击处理程序以记录按钮事件：

   ```javascript
   function onMouseDown(event) {
       if (onMouseMove(event)) {
           mIsButtonPressed[event.button] = true;
       }
   }
   ```

5. **定义鼠标释放处理程序**

   定义鼠标按钮释放处理程序，以便于检测鼠标按钮点击事件。回顾第 4 章中关于键盘输入的讨论，为了检测按钮释放事件，您应该测试之前已释放且当前被点击的按钮状态。`onMouseUp()` 处理程序记录鼠标按钮的释放状态。

   ```javascript
   function onMouseUp(event) {
       onMouseMove(event);
       mIsButtonPressed[event.button] = false;
   }
   ```

6. **修改 init() 函数以支持鼠标**

   修改 `init()` 函数以接收 `canvasID` 参数并初始化鼠标事件处理程序：

   ```javascript
   function init(canvasID) {
       let i;
       // 键盘支持
       // ... 与之前的代码相同 ...
       // 鼠标支持
       for (i = 0; i < 3; i++) {
           mButtonPreviousState[i] = false;
           mIsButtonPressed[i] = false;
           mIsButtonClicked[i] = false;
       }
       window.addEventListener('mousedown', onMouseDown);
       window.addEventListener('mouseup', onMouseUp);
       window.addEventListener('mousemove', onMouseMove);
       mCanvas = document.getElementById(canvasID);
   }
   ```

7. **修改 update() 函数以处理鼠标状态**

   修改 `update()` 函数以类似键盘的方式处理鼠标按钮状态变化。请注意鼠标点击条件：之前未点击的按钮现在被点击了。

   ```javascript
   function update() {
       let i;
       // 更新键盘输入状态
       // ... 与之前的代码相同 ...
       // 更新鼠标输入状态
       for (i = 0; i < 3; i++) {
           mIsButtonClicked[i] = (!mButtonPreviousState[i]) &&
               mIsButtonPressed[i];
           mButtonPreviousState[i] = mIsButtonPressed[i];
       }
   }
   ```

8. **定义鼠标状态访问器函数**

   定义函数以检索鼠标位置和鼠标按钮状态：

   ```javascript
   function isButtonPressed(button) {
       return mIsButtonPressed[button];
   }
   function isButtonClicked(button) {
       return mIsButtonClicked[button];
   }
   function getMousePosX() {
       return mMousePosX;
   }
   function getMousePosY() {
       return mMousePosY;
   }
   ```

9. **导出鼠标功能**

   最后，请记住导出新定义的功能：

   ```javascript
   export {
       keys,
       eMouseButton,
       init, cleanUp, update,
       // 键盘
       isKeyClicked, isKeyPressed,
       // 鼠标
       isButtonClicked, isButtonPressed, getMousePosX, getMousePosY
   };
   ```

## 修改相机以支持视口到 WC 空间的变换

`Camera` 类封装了 WC 窗口和视口，因此应负责转换鼠标位置。回顾一下，为了保持可读性，`Camera` 类的源代码文件是根据功能分离的。类的基本函数定义在 `camera_main.js` 中。`camera_manipulate.js` 文件从 `camera_main.js` 导入并定义额外的操作函数。最后，`camera.js` 文件从 `camera_manipulate.js` 导入以包含所有定义的函数，并导出 `Camera` 类供外部访问。

这种从后续源代码文件链式导入以定义额外函数的方式将继续用于 `Camera` 类，`camera_input.js` 将定义输入功能：

1. **创建 camera_input.js**

   在 `src/engine/cameras` 文件夹中创建一个新文件并将其命名为 `camera_input.js`。该文件将通过定义鼠标输入支持函数来扩展 `Camera` 类。导入以下文件：

   - `camera_manipulation.js` 以获取 `Camera` 类的所有已定义函数
   - `eViewport` 常量以访问视口数组
   - `input` 模块以访问鼠标相关函数

   ```javascript
   import Camera from "./camera_manipulation.js";
   import { eViewport } from "./camera_main.js";
   import * as input from "../input.js";
   // ... 实现如下 ...
   export default Camera;
   ```

2. **定义鼠标 DC 坐标函数**

   定义函数将鼠标位置从画布坐标空间转换到 DC 空间，如图 7-10 所示：

   ```javascript
   Camera.prototype._mouseDCX = function () {
       return input.getMousePosX() - this.mViewport[eViewport.eOrgX];
   }
   Camera.prototype._mouseDCY = function() {
       return input.getMousePosY() - this.mViewport[eViewport.eOrgY];
   }
   ```

3. **定义视口边界检查**

   定义一个函数来确定给定的鼠标位置是否在相机的视口边界内：

   ```javascript
   Camera.prototype.isMouseInViewport = function () {
       let dcX = this._mouseDCX();
       let dcY = this._mouseDCY();
       return ((dcX >= 0) && (dcX < this.mViewport[eViewport.eWidth]) &&
           (dcY >= 0) && (dcY < this.mViewport[eViewport.eHeight]));
   }
   ```

4. **定义鼠标 WC 坐标函数**

   定义函数将鼠标位置转换到 WC 空间，如图 7-11 所示：

   ```javascript
   Camera.prototype.mouseWCX = function () {
       let minWCX = this.getWCCenter()[0] - this.getWCWidth() / 2;
       return minWCX + (this._mouseDCX() *
           (this.getWCWidth() / this.mViewport[eViewport.eWidth]));
   }
   Camera.prototype.mouseWCY = function () {
       let minWCY = this.getWCCenter()[1] - this.getWCHeight() / 2;
       return minWCY + (this._mouseDCY() *
           (this.getWCHeight() / this.mViewport[eViewport.eHeight]));
   }
   ```

   最后，更新 `Camera` 类访问文件以正确导出新定义的输入功能。这通过编辑 `camera.js` 文件并将从 `camera_manipulate.js` 的导入替换为 `camera_input.js` 来完成：

   ```javascript
   import Camera from "./camera_input.js";
   export default Camera;
   ```

## 在 MyGame 中测试鼠标输入

