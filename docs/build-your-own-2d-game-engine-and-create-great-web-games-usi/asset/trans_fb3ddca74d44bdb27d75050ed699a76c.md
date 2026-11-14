

## 测试光源

必须修改 MyGame 关卡以使用并测试新定义的光源功能。

### 修改 Hero 和 Minion

修改 `Hero` 和 `Minion` 类以适配新的 `LightRenderable` 对象：

#### 步骤 1

编辑 `src/my_game/objects` 文件夹中的 `hero.js` 文件；在构造函数中，将 `SpriteRenderable` 替换为 `LightRenderable` 实例化：

```javascript
constructor(spriteTexture) {
    super(null);
    this.kDelta = 0.3;
    this.mRenderComponent = new engine.LightRenderable(spriteTexture);
    // ... identical to previous code ...
}
```

#### 步骤 2

编辑 `src/my_game/objects` 文件夹中的 `minion.js` 文件；在构造函数中，将 `SpriteRenderable` 替换为 `LightRenderable` 实例化：

```javascript
constructor(spriteTexture, atX, atY) {
    super(null);
    this.kDelta = 0.2;
    this.mRenderComponent = new engine.LightRenderable(spriteTexture);
    // ... identical to previous code ...
}
```

### 修改 MyGame 对象

随着光源实现的完成和游戏对象的妥善更新，你现在可以修改 MyGame 关卡以显示和测试光源。由于在 `my_game_main.js` 文件中为新对象添加变量、初始化对象、绘制对象和更新对象的代码更改既简单又重复，此处不再展示详细内容。

### 观察结果

项目现已完成，你可以运行它并检查结果。有几个观察点值得注意。首先是光源产生的照明效果看起来像一个圆圈。如图 [8-2](#334805_2_En_8_Chapter.xhtml#Fig2) 所示，这是点光源在 `z = 0` 平面（你的对象所在平面）上形成的照明圆圈。按 `Z` 或 `X` 键增加或减少光源的 `z` 位置，观察照明圆圈因相交面积变化而缩小和增大。当你继续增加/减少 `z` 位置时，可以验证球体/平面相交的结果。照明圆圈最终将开始缩小，当球体移离 `z=0` 平面的距离超过其半径时，圆圈将完全消失。

你也可以按 `C` 或 `V` 键增加或减少点光源半径，以增大或减小照明体积，并观察照明圆圈半径的相应变化。

现在，按 `WASD` 键并配合鼠标左键移动 Hero，观察点光源始终跟随 Hero 并正确照亮背景。请注意，光源照亮了左侧的 Minion、Hero 和背景，但**没有**照亮场景中的另外三个对象。这是因为右侧的 Minion 和红绿方块不是 `LightRenderable` 对象，因此无法被定义的光源照亮。

## 多光源与距离衰减

在之前的项目中，定义了一个具有照亮球形体积能力的单个点光源。这种类型的光源在许多游戏中很有用，但仅限于单个光源是一种限制。引擎应支持来自多个光源的照明，以满足不同游戏的设计需求。这一缺陷将在下一个项目中通过对多光源的通用支持得到解决。多光源的实现原则与之前的项目相同，修改点在于将单个光源替换为光源数组。如图 [8-5](#334805_2_En_8_Chapter.xhtml#Fig5) 所示，将定义一个新的 `Light` 对象，而 `LightRenderable` 对象将被修改以支持 `Light` 对象数组。`LightShader` 对象将定义一个 `ShaderLightAtIndex` 对象数组，它们能够将光源信息传递给 GLSL `light_fs` 片段着色器中的 `uLights` 数组，以进行照明计算。

![图 8-5 ¶ 多光源支持](images/334805_2_En_8_Chapter/334805_2_En_8_Fig5_HTML.png)

**图 8-5 ¶ 多光源支持**

之前项目的点光源照明结果可以得到改进。你可能已经观察到，在其边界处，照明圆圈会突然消失，亮度过渡生硬。这种照明效果的骤然消失不符合现实生活，在现实中，给定光源的效果会随距离逐渐减弱，而不是突然关闭。一个更具视觉愉悦感的光照明结果应显示一个照明圆圈，其边界处的照明效果逐渐消失。这种光照明效果随距离逐渐减弱的现象称为**距离衰减**。通常做法是用二次函数来近似远距离衰减，因为它们产生的效果类似于现实世界。一般来说，距离衰减可以用多种方式近似，且常针对游戏需求进行细化。

在下文中，你将实现近截断距离和远截断距离，即距离光源两个位置，距离衰减效果将在这两个位置开始和结束。这两个值让你能控制光源显示一个完全照明的中心区域，照明衰减仅在指定距离处发生。最后，将定义光强度，以允许在不改变颜色的情况下调暗光线。有了这些额外参数，就可以定义截然不同的效果。例如，你可以拥有一个柔和、几乎难以察觉且覆盖范围广的光源，或者一个过度饱和、发光且集中在场景小区域内的光源。

### 多光源项目

