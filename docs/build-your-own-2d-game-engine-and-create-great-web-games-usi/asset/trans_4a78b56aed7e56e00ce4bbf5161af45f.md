
## 代码实现

### `_initText()` 与 `init()` 方法

```javascript
_initText(font, posX, posY, color, textH) {
    font.setColor(color);
    font.getXform().setPosition(posX, posY);
    font.setTextHeight(textH);
}

init() {
    // Step A: set up the cameras
    this.mCamera = new engine.Camera(
        vec2.fromValues(50, 33),  // position of the camera
        100,                      // width of camera
        [0, 0, 600, 400]          // viewport (orgX, orgY, width, height)
    );
    this.mCamera.setBackgroundColor([0.8, 0.8, 0.8, 1]); // sets the background to gray

    // Step B: Create the font and minion images using sprite
    this.mFontImage = new engine.SpriteRenderable(this.kFontImage);
    this.mFontImage.setColor([1, 1, 1, 0]);
    this.mFontImage.getXform().setPosition(15, 50);
    this.mFontImage.getXform().setSize(20, 20);

    // The right minion
    this.mMinion = new engine.SpriteAnimateRenderable(this.kMinionSprite);
    this.mMinion.setColor([1, 1, 1, 0]);
    this.mMinion.getXform().setPosition(15, 25);
    this.mMinion.getXform().setSize(24, 19.2);
    this.mMinion.setSpriteSequence(
        512, 0,     // first element: top, left
        204, 164,   // width x height in pixels
        5,          // number of elements in this sequence
        0           // horizontal padding in between
    );
    this.mMinion.setAnimationType(engine.eAnimationType.eSwing);
    this.mMinion.setAnimationSpeed(15); // show each element for mAnimSpeed updates

    // Step D: Create hero object with texture from lower-left corner
    this.mHero = new engine.SpriteRenderable(this.kMinionSprite);
    this.mHero.setColor([1, 1, 1, 0]);
    this.mHero.getXform().setPosition(35, 50);
    this.mHero.getXform().setSize(12, 18);
    this.mHero.setElementPixelPositions(0, 120, 0, 180);

    // Create the fonts
    this.mTextSysFont = new engine.FontRenderable("System Font: in Red");
    this._initText(this.mTextSysFont, 50, 60, [1, 0, 0, 1], 3);

    this.mTextCon16 = new engine.FontRenderable("Consolas 16: in black");
    this.mTextCon16.setFontName(this.kFontCon16);
    this._initText(this.mTextCon16, 50, 55, [0, 0, 0, 1], 2);

    this.mTextCon24 = new engine.FontRenderable("Consolas 24: in black");
    this.mTextCon24.setFontName(this.kFontCon24);
    this._initText(this.mTextCon24, 50, 50, [0, 0, 0, 1], 3);

    this.mTextCon32 = new engine.FontRenderable("Consolas 32: in white");
    this.mTextCon32.setFontName(this.kFontCon32);
    this._initText(this.mTextCon32, 50, 40, [1, 1, 1, 1], 4);

    this.mTextCon72 = new engine.FontRenderable("Consolas 72: in blue");
    this.mTextCon72.setFontName(this.kFontCon72);
    this._initText(this.mTextCon72, 30, 30, [0, 0, 1, 1], 6);

    this.mTextSeg96 = new engine.FontRenderable("Segment7-92");
    this.mTextSeg96.setFontName(this.kFontSeg96);
    this._initText(this.mTextSeg96, 30, 15, [1, 1, 0, 1], 7);

    this.mTextToWork = this.mTextCon16;
}
```

### 修改 `update()` 函数

```javascript
update() {
    // ... identical to previous code ...
    // choose which text to work on
    if (engine.input.isKeyClicked(engine.input.keys.Zero)) {
        this.mTextToWork = this.mTextCon16;
    }
    if (engine.input.isKeyClicked(engine.input.keys.One)) {
        this.mTextToWork = this.mTextCon24;
    }
    if (engine.input.isKeyClicked(engine.input.keys.Three)) {
        this.mTextToWork = this.mTextCon32;
    }
    if (engine.input.isKeyClicked(engine.input.keys.Four)) {
        this.mTextToWork = this.mTextCon72;
    }
    let deltaF = 0.005;
    if (engine.input.isKeyPressed(engine.input.keys.Up)) {
        if (engine.input.isKeyPressed(engine.input.keys.X)) {
            this.mTextToWork.getXform().incWidthBy(deltaF);
        }
        if (engine.input.isKeyPressed(engine.input.keys.Y)) {
            this.mTextToWork.getXform().incHeightBy(deltaF);
        }
        this.mTextSysFont.setText(
            this.mTextToWork.getXform().getWidth().toFixed(2) + "x" +
            this.mTextToWork.getXform().getHeight().toFixed(2));
    }
    if (engine.input.isKeyPressed(engine.input.keys.Down)) {
        if (engine.input.isKeyPressed(engine.input.keys.X)) {
            this.mTextToWork.getXform().incWidthBy(-deltaF);
        }
        if (engine.input.isKeyPressed(engine.input.keys.Y)) {
            this.mTextToWork.getXform().incHeightBy(-deltaF);
        }
        this.mTextSysFont.setText(
            this.mTextToWork.getXform().getWidth().toFixed(2) + "x" +
            this.mTextToWork.getXform().getHeight().toFixed(2));
    }
}
```

上述代码展示了运行时可执行的以下操作：

1. 根据键盘 `0` 到 `4` 的输入，选择要操作的 `FontRenderable` 对象。
2. 当同时按下左/右方向键与 `X`/`Y` 键时，控制选中 `FontRenderable` 对象的宽度和高度。

现在你可以与 `Font Support` 项目交互，修改每个显示字体消息的大小，并让英雄向左右移动。

## 总结

在本章中，你学习了如何将图像粘贴或纹理映射到单位正方形上，以更好地表示游戏中的对象。你还学习了如何识别图像的选定子区域，并基于归一化范围的纹理坐标系将纹理映射到单位正方形。随后本章解释了精灵表如何减少加载纹理图像所需的时间，同时便于创建动画。这些知识随后被推广并应用于位图字体的绘制。

纹理映射和精灵表渲染的实现利用了游戏引擎架构的一个重要方面：`SimpleShader`/`Renderable` 对象对。其中 JavaScript `SimpleShader` 对象被定义为与相应的 GLSL 着色器接口，而 `Renderable` 对象则便于创建和交互多个对象实例。例如，你创建了 `TextureShader` 来与 `TextureVS` 和 `TextureFS` GLSL 着色器接口，并创建了 `TextureRenderable` 供游戏程序员使用。这一模式在 `SpriteShader` 和 `SpriteRenderable` 中重复出现。`SpriteShader` 对象与 `SpriteAnimateRenderable` 的搭配经验表明，在适当情况下，同一个着色器对象可以支持游戏引擎中的多种可渲染对象类型。这种 `SimpleShader`/`Renderable` 对实现模式将在第 8 章再次出现，届时你将学习创建 3D 光照效果。

在本章开始时，你的游戏引擎支持玩家通过键盘操作对象，并以各种大小和方向绘制这些对象。有了本章的功能，你现在可以用有趣的图像表示这些对象，并在需要时创建这些对象的动画。在下一章中，你将学习定义和支持这些对象的行为，包括追逐和碰撞检测等伪自主行为。

### 游戏设计考量
