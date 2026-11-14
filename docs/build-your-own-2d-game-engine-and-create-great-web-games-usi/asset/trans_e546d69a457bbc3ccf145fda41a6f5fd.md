

本项目演示了如何在单个场景中实现多个点光源。它还演示了如何提升点光源模型的复杂度，使其更加灵活，从而服务于更广泛的用途。您可以在图 [8-6](#334805_2_En_8_Chapter.xhtml#Fig6) 中看到该项目运行的示例。该项目的源代码位于 `chapter8/8.3.multiple_lights` 文件夹中。

![图 8-6 ¶ 运行多光源项目](images/334805_2_En_8_Chapter/334805_2_En_8_Fig6_HTML.jpg)

**图 8-6 ¶ 运行多光源项目**

项目控制方式如下：

*   **WASD 键**：移动屏幕上的主角角色
*   **数字键 0、1、2 和 3**：选择对应的光源
*   **方向键**：移动当前选中的光源
*   **Z/X 键**：增加/减小光源的 z 位置
*   **C/V 和 B/N 键**：增加/减小选中光源的近裁剪和远裁剪距离
*   **K/L 键**：增加/减小选中光源的强度
*   **H 键**：切换选中光源的开/关状态

项目目标如下：

*   在引擎和 GLSL 着色器中构建支持多光源的基础设施
*   理解并检验光照的距离衰减效果
*   体验在场景中控制和操纵多个光源

## 修改 GLSL 光照片元着色器

`light_fs` 片元着色器需要修改以支持距离衰减、裁剪范围和多光源：

1.  在 `light_fs.glsl` 文件中，移除为单个光源添加的光照变量，并添加一个用于保存光照信息的结构体，该结构体包含位置、颜色、近距离、远距离、强度和开/关变量。定义好结构体后，向片元着色器添加一个光源统一变量数组。请注意，已添加一个 `#define` 来保存要使用的光源数量。

> **注意**
>
> GLSL 要求数组大小和循环迭代次数必须是常量。`kGLSLuLightArraySize` 是光源数组大小和相应循环迭代控制的常量。请随意更改此值，以定义硬件所能支持的任意数量的光源。例如，您可以尝试将光源数量增加到 50，然后测试和衡量性能。

```glsl
// 光照信息
#define kGLSLuLightArraySize 4     // GLSL 片元着色器要求循环控制
                                   // 变量为常数。这个数字 4 表示，
                                   // 该片元着色器将 _始终_ 处理
                                   // 所有 4 个光源。
                                   // **********警告**************
                                   // 此数字必须与 LightShader.js 文件中定义的
                                   // 同名常量对应。
                                   // **********警告**************
                                   // 更改此数字时，请务必：更新
                                   // LightShader.js 文件中定义的
                                   // kGLSLuLightArraySize。
struct Light {
    vec3 Position;   // 以像素空间为单位！
    vec4 Color;
    float Near;      // 以像素空间为单位的距离
    float Far;       // 以像素空间为单位的距离
    float Intensity;
    bool IsOn;
};
uniform Light uLights[kGLSLuLightArraySize];     // 此着色器支持的最大光源数组
```

2.  定义 `LightEffect()` 函数以计算来自光源的照明结果。该函数利用光源与当前像素之间的距离，判断像素位于近半径内、近远半径之间，还是远半径之外。如果像素位置位于近半径内，则无衰减，强度设为 1。如果位置位于近半径和远半径之间，则强度由二次函数调制。大于远半径的距离将导致相应光源不产生任何照明，即强度为 0。

```glsl
vec4 LightEffect(Light lgt) {
    vec4 result = vec4(0);
    float strength = 0.0;
    float dist = length(lgt.Position.xyz - gl_FragCoord.xyz);
    if (dist <= lgt.Far) {
        if (dist <= lgt.Near)
            strength = 1.0;  // 无衰减
        else {
            // 简单的二次衰减
            float n = dist - lgt.Near;
            float d = lgt.Far - lgt.Near;
            strength = smoothstep(0.0, 1.0, 1.0-(n*n)/(d*d));
                            // 混合衰减
        }
    }
    result = strength * lgt.Intensity * lgt.Color;
    return result;
}
```

3.  修改 `main()` 函数以遍历所有定义的光源，并调用 `LightEffect()` 函数计算和累加数组中相应光源的贡献：

```glsl
void main(void) {
    // 基于 uPixelColor 设置的简单着色
    vec4 textureMapColor = texture2D(uSampler,
                                     vec2(vTexCoord.s, vTexCoord.t));
    vec4 lgtResults = uGlobalAmbientIntensity * uGlobalAmbientColor;
    // 现在决定是否由光源照明
    if (textureMapColor.a > 0.0) {
        for (int i=0; i<kGLSLuLightArraySize; i++) {
            if (uLights[i].IsOn) {
                lgtResults += LightEffect(uLights[i]);
            }
        }
    }
    lgtResults *= textureMapColor;
    ... 与之前的代码相同 ...
}
```

## 修改 Light 类

游戏引擎 `Light` 对象必须修改以反映 `light_fs` 片元着色器中新增的属性：近衰减、远衰减和强度。

1.  修改 `Lights.js` 构造函数以定义新属性的变量：

```javascript
constructor() {
    this.mColor = vec4.fromValues(0.1, 0.1, 0.1, 1);  // 光照颜色
    this.mPosition = vec3.fromValues(0, 0, 5); // 世界坐标系下的光源位置
    this.mNear = 5;   // 世界坐标系下的有效半径
    this.mFar = 10;   // 近距离内全亮，远距离外关闭
    this.mIntensity = 1;
    this.mIsOn = true;
}
```

2.  为这些变量定义相应的 get 和 set 访问器。请注意，radius 变量已被泛化，并由近裁剪和远裁剪距离取代。

```javascript
setNear(n) { this.mNear = n; }
getNear() { return this.mNear; }
setFar(f) { this.mFar = f; }
getFar() { return this.mFar; }
setIntensity(i) { this.mIntensity = i; }
getIntensity() { return this.mIntensity; }
setLightTo(on) { this.mIsOn = on; }
```

## 定义 LightSet 类

您将定义一个 `LightSet` 类，以便于处理 `Light` 对象的集合。在 `src/engine/lights` 文件夹中，创建一个新文件并命名为 `light_set.js`。定义用于操作 `Light` 对象集合的基本接口。

```javascript
class LightSet {
    constructor() { this.mSet = []; }
    numLights() { return this.mSet.length; }
    getLightAt(index) { return this.mSet[index]; }
    addToSet(light) { this.mSet.push(light); }
}
export default LightSet;
```

最后，别忘了导出该类，并记得更新引擎访问文件 `index.js`，将新定义的功能转发给客户端。

## 定义 ShaderLightAt 类

定义 `ShaderLightAt` 类，将 `Light` 对象的信息发送给 `light_fs` GLSL 片元着色器中 `uLights` 数组的一个元素：

1.  在 `src/engine/shaders` 文件夹中，创建一个新文件并命名为 `shader_light_at.js`；定义 `ShaderLightAt` 类及其构造函数，接收一个着色器和 `uLight` 数组的索引。别忘了导出该类。

```javascript
import * as glSys from "../core/gl.js";
class ShaderLightAt {
    constructor(shader, index) {
        this._setShaderReferences(shader, index);
    }
    ... 后续实现 ...
}
export default ShaderLightAt;
```

2.  实现 `_setShaderReferences()` 函数，将光源属性引用设置为 `light_fs` 片元着色器中 `uLights` 数组的特定索引：

