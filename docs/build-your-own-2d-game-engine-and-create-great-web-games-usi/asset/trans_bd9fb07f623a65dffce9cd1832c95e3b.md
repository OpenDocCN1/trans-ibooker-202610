

*   理解并解决单个粒子位置与 `RigidShape` 对象之间的碰撞
*   构建一个支持与 `RigidShape` 交互的粒子引擎组件

## 修改粒子系统

通过精心设计的基础设施，新功能的实现可以被局部化。就粒子碰撞而言，所有修改都在 `src/engine/components` 文件夹下的 `particle_system.js` 文件中。

### 1. 编辑 `particle_system.js` 以定义并初始化临时局部变量

编辑 `particle_system.js` 以定义并初始化用于解决与 `RigidShape` 对象碰撞的临时局部变量。`mCircleCollider` 对象将用于在碰撞中代表单个粒子。

```
import Transform from "../utils/transform.js";
import RigidCircle from "../rigid_shapes/rigid_circle.js";
import CollisionInfo from "../rigid_shapes/collision_info.js";

let mXform = null;        // for collision with rigid shapes
let mCircleCollider = null;
let mCollisionInfo = null;
let mFrom1to2 = [0, 0];

function init() {
    mXform = new Transform();
    mCircleCollider = new RigidCircle(mXform, 1.0);
    mCollisionInfo = new CollisionInfo();
}
```

### 2. 定义 `resolveCirclePos()` 函数

定义 `resolveCirclePos()` 函数，通过将位置推到圆形外部来解决 `RigidCircle` 与某位置之间的碰撞：

```
function resolveCirclePos(circShape, particle) {
    let collision = false;
    let pos = particle.getPosition();
    let cPos = circShape.getCenter();
    vec2.subtract(mFrom1to2, pos, cPos);
    let dist = vec2.length(mFrom1to2);
    if (dist < circShape.getRadius()) {
        vec2.scale(mFrom1to2, mFrom1to2, 1/dist);
        vec2.scaleAndAdd(pos, cPos, mFrom1to2, circShape.getRadius());
        collision = true;
    }
    return collision;
}
```

### 3. 定义 `resolveRectPos()` 函数

定义 `resolveRectPos()` 函数，通过将 `mCircleCollider` 局部变量包裹在该位置周围并调用 `RigidCircle` 对 `RigidRectangle` 的碰撞函数，来解决 `RigidRectangle` 与某位置之间的碰撞。当检测到相互渗透时，根据计算出的 `mCollisionInfo` 将该位置推到矩形外部。

```
function resolveRectPos(rectShape, particle) {
    let collision = false;
    let s = particle.getSize();
    let p = particle.getPosition();
    mXform.setSize(s[0], s[1]); // referred by mCircleCollision
    mXform.setPosition(p[0], p[1]);
    if (mCircleCollider.boundTest(rectShape)) {
        if (rectShape.collisionTest(mCircleCollider, mCollisionInfo)) {
            // make sure info is always from rect towards particle
            vec2.subtract(mFrom1to2,
                mCircleCollider.getCenter(), rectShape.getCenter());
            if (vec2.dot(mFrom1to2, mCollisionInfo.getNormal()) < 0)
                mCircleCollider.adjustPositionBy(
                    mCollisionInfo.getNormal(), -mCollisionInfo.getDepth());
            else
                mCircleCollider.adjustPositionBy(
                    mCollisionInfo.getNormal(), mCollisionInfo.getDepth());
            p = mXform.getPosition();
            particle.setPosition(p[0], p[1]);
            collision = true;
        }
    }
    return collision;
}
```

### 4. 实现 `resolveRigidShapeCollision()` 和 `resolveRigidShapeSetCollision()`

实现 `resolveRigidShapeCollision()` 和 `resolveRigidShapeSetCollision()`，以便客户端游戏开发者能方便地调用。这些函数解决单个或一组 `RigidShape` 对象与 `ParticleSet` 对象之间的碰撞。

```
// obj: a GameObject (with potential mRigidBody)
// pSet: set of particles (ParticleSet)
function resolveRigidShapeCollision(obj, pSet) {
    let i, j;
    let collision = false;
    let rigidShape = obj.getRigidBody();
    for (j = 0; j < pSet.size(); j++) {
        if (rigidShape.getType() == "RigidRectangle")
            collision = resolveRectPos(rigidShape, pSet.getObjectAt(j));
        else if (rigidShape.getType() == "RigidCircle")
            collision = resolveCirclePos(rigidShape, pSet.getObjectAt(j));
    }
    return collision;
}

// objSet: set of GameObjects (with potential mRigidBody)
// pSet: set of particles (ParticleSet)
function resolveRigidShapeSetCollision(objSet, pSet) {
    let i, j;
    let collision = false;
    if ((objSet.size === 0) || (pSet.size === 0))
        return false;
    for (i = 0; i < objSet.size(); i++) {
        let rigidShape = objSet.getObjectAt(i).getRigidBody();
        for (j = 0; j < pSet.size(); j++) {
            if (rigidShape.getType() == "RigidRectangle")
                collision = resolveRectPos(rigidShape,
                    pSet.getObjectAt(j)) || collision;
            else if (rigidShape.getType() == "RigidCircle")
                collision = resolveCirclePos(rigidShape,
                    pSet.getObjectAt(j)) || collision;
        }
    }
    return collision;
}
```

### 5. 导出新定义的函数

最后，别忘了导出新定义的函数：

```
export {init,
    getSystemAcceleration, setSystemAcceleration,
    resolveRigidShapeCollision, resolveRigidShapeSetCollision}
```

## 初始化粒子系统

`particle_system.js` 中定义的临时变量必须在游戏循环开始前初始化。编辑 `loop.js`，从 `particle_system.js` 导入，并在 `start()` 函数中异步加载完成后调用 `init()` 函数。

```
... identical to previous code ...
import * as debugDraw from "./debug_draw.js";
import * as particleSystem from "../components/particle_system.js";
... identical to previous code ...

async function start(scene) {
    ... identical to previous code ...
    // Wait for any async requests before game-load
    await map.waitOnPromises();
    // system init that can only occur after all resources are loaded
    particleSystem.init();
    ... identical to previous code ...
}
```

## 测试粒子系统

`MyGame` 类所需的修改非常简单。必须定义一个新变量以支持碰撞解析的切换，并在 `my_game_main.js` 中按如下方式修改 `update()` 函数：

```
update() {
    ... identical to previous code ...
    if (engine.input.isKeyClicked(engine.input.keys.One))
        this.mPSCollision = !this.mPSCollision;
    if (this.mPSCollision) {
        engine.particleSystem.resolveRigidShapeSetCollision(
            this.mAllObjs, this.mParticles);
        engine.particleSystem.resolveRigidShapeSetCollision(
            this.mPlatforms, this.mParticles);
    }
    ... identical to previous code ...
}
```

## 观察

与之前的项目一样，你可以运行项目并使用 `Q` 和 `E` 键创建粒子。然而，请注意生成的粒子不会与任何对象重叠。你甚至可以尝试将鼠标指针移至某个 `RigidShape` 对象的边界内，然后按 `Q` 键。请注意，在所有情况下，粒子都是在形状外部生成的。

你可以尝试按 `1` 键来切换与刚体形状的碰撞。请注意，启用碰撞后，粒子看起来有点像火焰或爆炸中的琥珀色粒子，它们会在场景中弹离 `RigidShape` 对象的表面。当碰撞关闭时，正如你在上一个项目中观察到的，粒子似乎在其他物体前方燃烧或爆炸。这样，碰撞就只是控制粒子系统与游戏引擎其余部分集成的另一个参数而已。

你可能会觉得一直按着 `Q` 键生成粒子很麻烦。在下一个项目中，你将学习在固定时间段内生成粒子。

## 粒子发射器

