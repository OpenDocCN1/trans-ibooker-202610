
# 创建 GameObjectSet 类

在 `src/engine/game_objects` 文件夹中创建一个新文件，命名为 `game_object_set.js`。定义 `GameObjectSet` 类及其构造函数，初始化一个数组用于存放 `GameObject` 实例。

```javascript
class GameObjectSet {
    constructor() {
        this.mSet = [];
    }
    // ... implementation to follow ...
    export default GameObjectSet;
}
```

## 定义集合管理函数

```javascript
size() { return this.mSet.length; }
getObjectAt(index) { return this.mSet[index]; }
addToSet(obj) { this.mSet.push(obj); }
removeFromSet(obj) {
    let index = this.mSet.indexOf(obj);
    if (index > -1)
        this.mSet.splice(index, 1);
}
```

## 定义更新和绘制函数

更新和绘制集合中的每个 `GameObject` 实例：

```javascript
update() {
    let i;
    for (i = 0; i < this.mSet.length; i++) {
        this.mSet[i].update();
    }
}
draw(aCamera) {
    let i;
    for (i = 0; i < this.mSet.length; i++) {
        this.mSet[i].draw(aCamera);
    }
}
```

# 导出类到客户端

将新功能集成到引擎的最后一步是修改引擎访问文件 `index.js`。编辑 `index.js` 并添加以下导入和导出语句，以授予客户端访问 `GameObject` 和 `GameObjectSet` 类的权限：

```javascript
// ... identical to previous code ...
// game objects
import GameObject from "./game_objects/game_object.js";
import GameObjectSet from "./game_objects/game_object_set.js";
// ... identical to previous code ...
export default {
    // ... identical to previous code ...
    // Game Objects
    GameObject, GameObjectSet,
    // ... identical to previous code ...
}
```

> **注意**
>
> 通过引擎访问文件 `index.js` 导入/导出类的过程必须为每个新定义的功能重复执行。从现在起，将只提供提醒，不再展示简单的代码更改。

# 测试 GameObject 和 GameObjectSet

本项目的目标是确保新 `GameObject` 类正常运行，演示通过单个对象类型自定义行为，并观察更清晰的 `MyGame` 实现，明确反映游戏内逻辑。为实现这些目标，定义了三种对象类型：`DyePack`、`Hero` 和 `Minion`。在开始检查这些对象的详细实现之前，遵循良好的源代码组织实践，在 `src/my_game/objects` 中创建一个新文件夹用于存储新对象类型。

## DyePack GameObject

`DyePack` 类继承自 `GameObject` 类，演示最基本的 `GameObject` 示例：一个没有行为、仅绘制到屏幕的对象。

在 `src/my_game/objects` 文件夹中创建一个新文件，命名为 `dye_pack.js`。从引擎访问文件 `index.js` 导入以获取所有游戏引擎功能。将 `DyePack` 定义为 `GameObject` 的子类，并按如下方式实现构造函数：

```javascript
import engine from "../../engine/index.js";
class DyePack extends engine.GameObject {
    constructor(spriteTexture) {
        super(null);
        this.kRefWidth = 80;
        this.kRefHeight = 130;
        this.mRenderComponent =
            new engine.SpriteRenderable(spriteTexture);
        this.mRenderComponent.setColor([1, 1, 1, 0.1]);
        this.mRenderComponent.getXform().setPosition(50, 33);
        this.mRenderComponent.getXform().setSize(
            this.kRefWidth / 50, this.kRefHeight / 50);
        this.mRenderComponent.setElementPixelPositions(510, 595, 23, 153);
    }
}
export default DyePack;
```

请注意，即使没有特定行为，`DyePack` 也在实现过去位于 `MyGame` 关卡 `init()` 函数中的代码。这样，`DyePack` 对象隐藏了特定的几何信息并简化了 `MyGame` 关卡。

> **注意**
>
> 几乎所有客户端源代码文件都需要从引擎访问文件 `index.js` 导入，这一点将不再重复。

## Hero GameObject

`Hero` 类支持直接的用户键盘控制。该对象演示了从 `MyGame` 的 `update()` 函数中隐藏游戏对象控制逻辑。

### 1. 创建 Hero 类

在 `src/my_game/objects` 文件夹中创建一个新文件，命名为 `hero.js`。将 `Hero` 定义为 `GameObject` 的子类，并实现构造函数以初始化精灵 UV 值、大小和位置。确保导出并共享此类。

```javascript
class Hero extends engine.GameObject {
    constructor(spriteTexture) {
        super(null);
        this.kDelta = 0.3;
        this.mRenderComponent =
            new engine.SpriteRenderable(spriteTexture);
        this.mRenderComponent.setColor([1, 1, 1, 0]);
        this.mRenderComponent.getXform().setPosition(35, 50);
        this.mRenderComponent.getXform().setSize(9, 12);
        this.mRenderComponent.setElementPixelPositions(0, 120, 0, 180);
    }
    // ... implementation to follow ...
    export default Hero;
}
```

### 2. 添加更新函数

添加一个函数以支持通过用户键盘控制更新此对象。`Hero` 对象根据键盘的 WASD 输入以 `kDelta` 速率移动。

```javascript
update() {
    // control by WASD
    let xform = this.getXform();
    if (engine.input.isKeyPressed(engine.input.keys.W)) {
        xform.incYPosBy(this.kDelta);
    }
    if (engine.input.isKeyPressed(engine.input.keys.S)) {
        xform.incYPosBy(-this.kDelta);
    }
    if (engine.input.isKeyPressed(engine.input.keys.A)) {
        xform.incXPosBy(-this.kDelta);
    }
    if (engine.input.isKeyPressed(engine.input.keys.D)) {
        xform.incXPosBy(this.kDelta);
    }
}
```

## Minion GameObject

`Minion` 类演示了简单的自主行为也可以被隐藏。

### 1. 创建 Minion 类

在 `src/my_game/objects` 文件夹中创建一个新文件，命名为 `minion.js`。将 `Minion` 定义为 `GameObject` 的子类，并实现构造函数以初始化精灵 UV 值、精灵动画参数、大小和位置，如下所示：

```javascript
class Minion extends engine.GameObject {
    constructor(spriteTexture, atY) {
        super(null);
        this.kDelta = 0.2;
        this.mRenderComponent =
            new engine.SpriteAnimateRenderable(spriteTexture);
        this.mRenderComponent.setColor([1, 1, 1, 0]);
        this.mRenderComponent.getXform().setPosition(
            Math.random() * 100, atY);
        this.mRenderComponent.getXform().setSize(12, 9.6);
        // first element pixel position: top-left 512 is top of image
        // 0 is left of the image
        this.mRenderComponent.setSpriteSequence(512, 0,
            204, 164,   // width x height in pixels
            5,          // number of elements in this sequence
            0);         // horizontal padding in between
        this.mRenderComponent.setAnimationType(
            engine.eAnimationType.eSwing);
        this.mRenderComponent.setAnimationSpeed(15);
        // show each element for mAnimSpeed updates
    }
    // ... implementation to follow ...
}
export default Minion;
```

### 2. 添加更新函数

添加一个函数以更新精灵动画，支持简单的从右向左移动，并提供环绕功能：

```javascript
update() {
    // remember to update this.mRenderComponent's animation
    this.mRenderComponent.updateAnimation();
    // move towards the left and wraps
    let xform = this.getXform();
    xform.incXPosBy(-this.kDelta);
    // if fly off to the left, re-appear at the right
    if (xform.getXPos() < 0) {
        xform.setXPos(100);
        xform.setYPos(65 * Math.random());
    }
}
```

## MyGame 场景

与所有情况一样，`MyGame` 关卡在 `my_game.js` 文件中实现。定义了三个特定的 `GameObject` 子类后，请遵循以下步骤：

### 1. 导入新定义的对象

除了引擎访问文件 `index.js` 外，为了访问新定义的对象，必须导入相应的源代码：
