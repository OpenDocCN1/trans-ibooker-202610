
在 2D 游戏中平铺背景时，重要的是要认识到只需要绘制覆盖相机 WC 边界的瓦片。这在图 [11-3](#334805_2_En_11_Chapter.xhtml#Fig3) 中有所说明。在该示例中，要平铺的背景对象定义在 WC 原点，具有自己的宽度和高度。然而，在这种情况下，相机 WC 边界与定义的背景对象不相交。图 [11-3](#334805_2_En_11_Chapter.xhtml#Fig3) 显示背景对象需要平铺六次才能覆盖相机 WC 边界。请注意，由于通过相机不可见，原点处的玩家定义背景对象不需要绘制。

![图 11-3 ¶ 为相机 WC 边界生成平铺背景](images/334805_2_En_11_Chapter/334805_2_En_11_Fig3_HTML.png)

**图 11-3 ¶ 为相机 WC 边界生成平铺背景**

对于给定的背景对象和相机 WC 边界，计算所需平铺的方法有很多。一种简单的方法是确定覆盖 WC 边界左下角的瓦片位置，然后在正 x 和 y 方向上平铺。

## 平铺对象项目

该项目演示了如何实现简单的背景平铺。您可以在图 [11-4](#334805_2_En_11_Chapter.xhtml#Fig4) 中看到该项目运行的示例。该项目的源代码定义在 `chapter11/11.1.tiled_objects` 文件夹中。

![图 11-4 ¶ 运行平铺对象项目](images/334805_2_En_11_Chapter/334805_2_En_11_Fig4_HTML.jpg)

**图 11-4 ¶ 运行平铺对象项目**

项目控制如下：

*   **WASD 键**：移动 Dye 角色（英雄）以平移 WC 窗口边界

项目目标如下：

*   体验使用多层背景
*   实现针对相机 WC 窗口边界的背景对象平铺

您可以在 `assets` 文件夹中找到以下外部资源。`fonts` 文件夹包含默认系统字体和六个纹理图像：`minion_sprite.png`、`minion_sprite_normal.png`、`bg.png`、`bg_normal.png`、`bg_layer.png` 和 `bg_layer_normal.png`。`Hero` 和 `Minion` 对象由 `minion_sprite.png` 图像中的精灵元素表示，`bg.png` 和 `bg_layer.png` 是两层背景图像。对应的 `_normal` 文件是法线贴图。

### 定义 TiledGameObject

回顾一下，`GameObject` 抽象了游戏中对象的基本行为，其外观由它引用的 `Renderable` 对象决定。`TiledGameObject` 是一种 `GameObject`，能够平铺引用的 `Renderable` 对象以覆盖给定 `Camera` 对象的 WC 边界。

1.  **创建文件**  
    在 `src/engine/game_objects` 文件夹中创建一个新文件，命名为 `tiled_game_object.js`。添加以下代码来构建对象。`mShouldTile` 变量提供了停止平铺过程的选项。

```javascript
class TiledGameObject extends GameObject {
    constructor(renderableObj) {
        super(renderableObj);
        this.mShouldTile = true; // can switch this off if desired
    }
    // … implementation to follow …
}
export default TiledGameObject;
```

2.  **定义 getter 和 setter**  
    为 `mShouldTile` 定义 getter 和 setter 函数：

```javascript
setIsTiled(t) { this.mShouldTile = t; }
shouldTile() { return this.mShouldTile; }
```

3.  **定义平铺绘制函数**  
    定义函数以平铺并绘制 `Renderable` 对象，覆盖 `aCamera` 对象的 WC 边界：

```javascript
_drawTile(aCamera) {
    // **Step A:** Compute the positions and dimensions of tiling object.
    let xf = this.getXform();
    let w = xf.getWidth();
    let h = xf.getHeight();
    let pos = xf.getPosition();
    let left = pos[0] - (w / 2);
    let right = left + w;
    let top = pos[1] + (h / 2);
    let bottom = top - h;
    // **Step B:** Get WC positions and dimensions of the drawing camera.
    let wcPos = aCamera.getWCCenter();
    let wcLeft = wcPos[0] - (aCamera.getWCWidth() / 2);
    let wcRight = wcLeft + aCamera.getWCWidth();
    let wcBottom = wcPos[1] - (aCamera.getWCHeight() / 2);
    let wcTop = wcBottom + aCamera.getWCHeight();
    // **Step C:** Determine offset to camera window's lower left corner.
    let dx = 0, dy = 0; // offset to the lower left corner
    // left/right boundary?
    if (right < wcLeft) { // left of WC left
        dx = Math.ceil((wcLeft - right) / w) * w;
    } else {
        if (left > wcLeft) { // not touching the left side
            dx = -Math.ceil((left - wcLeft) / w) * w;
        }
    }
    // top/bottom boundary
    if (top < wcBottom) { // Lower than the WC bottom
        dy = Math.ceil((wcBottom - top) / h) * h;
    } else {
        if (bottom > wcBottom) {  // not touching the bottom
            dy = -Math.ceil((bottom - wcBottom) / h) * h;
        }
    }
    // **Step D:** Save the original position of the tiling object.
    let sX = pos[0];
    let sY = pos[1];
    // **Step E:** Offset tiling object and update related position variables
    xf.incXPosBy(dx);
    xf.incYPosBy(dy);
    right = pos[0] + (w / 2);
    top = pos[1] + (h / 2);
    // **Step F:** Determine number of times to tile in x and y directions.
    let nx = 1, ny = 1; // times to draw in the x and y directions
    nx = Math.ceil((wcRight - right) / w);
    ny = Math.ceil((wcTop - top) / h);
    // **Step G:** Loop through each location to draw a tile
    let cx = nx;
    let xPos = pos[0];
    while (ny >= 0) {
        cx = nx;
        pos[0] = xPos;
        while (cx >= 0) {
            this.mRenderComponent.draw(aCamera);
            xf.incXPosBy(w);
            --cx;
        }
        xf.incYPosBy(h);
        --ny;
    }
    // **Step H:** Reset the tiling object to its original position.
    pos[0] = sX;
    pos[1] = sY;
}
```

`_drawTile()` 函数计算并重新定位 `Renderable` 对象以覆盖相机 WC 边界的左下角，并在正 x 和 y 方向上平铺该对象。请注意以下几点：

1.  **Step A 和 Step B** 计算平铺对象和相机 WC 边界的位置和尺寸。
2.  **Step C** 计算 `dx` 和 `dy` 偏移量，将 `Renderable` 对象平移以使其边界覆盖 `aCamera` WC 边界的左下角。对 `Math.ceil()` 函数的调用确保计算出的 `dx` 和 `dy` 是 `Renderable` 宽度和高度的整数倍。这对于确保平铺过程中没有重叠或间隙非常重要。
3.  **Step D** 在偏移和绘制之前保存 `Renderable` 对象的原始位置。**Step E** 将 `Renderable` 对象偏移以覆盖相机 WC 边界的左下角。
4.  **Step F** 计算所需的重复次数，**Step G** 在正 x 和 y 方向上平铺 `Renderable` 对象，直到结果覆盖整个相机 WC 边界。对 `Math.ceil()` 函数的调用确保计算出的 `nx` 和 `ny`（x 和 y 方向的平铺次数）是整数。
5.  **Step H** 将平铺对象的位置重置为原始位置。

4.  **重写 draw() 函数**  
    重写 `draw()` 函数，在启用平铺时调用 `_drawTile()` 函数：

```javascript
draw(aCamera) {
    if (this.isVisible() && (this.mDrawRenderable)) {
        if (this.shouldTile()) {
            // find out where we should be drawing
            this._drawTile(aCamera);
        } else {
            this.mRenderComponent.draw(aCamera);
        }
    }
}
```

最后，记得更新引擎访问文件 `index.js`，将新定义的功能转发给客户端。

### 修改 MyGame 以测试平铺对象
