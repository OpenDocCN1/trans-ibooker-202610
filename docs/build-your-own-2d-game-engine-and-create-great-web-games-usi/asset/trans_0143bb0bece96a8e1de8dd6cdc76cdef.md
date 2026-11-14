

# 资源映射与着色器加载器项目

本项目将指导你开发 `resource_map` 组件，这是一个用于资源管理的基础设施模块，并演示如何使用该模块异步加载着色器文件。你可以在图 4-3 中看到该项目运行的示例。该项目乍看之下与前一个项目完全相同，唯一的区别在于 GLSL 着色器的加载方式。本项目的源代码定义在 `chapter4/4.3.resource_map_and_shader_loader` 文件夹中。

![图 4-3 ¶ 运行资源映射与着色器加载器项目](images/334805_2_En_4_Chapter/334805_2_En_4_Fig3_HTML.jpg)

**图 4-3** ¶ 运行资源映射与着色器加载器项目

## 项目概览

本项目的控制方式与前一个项目完全相同，如下所示：

- **右箭头键**：将白色方块向右移动，并从游戏窗口左侧环绕回来
- **上箭头键**：旋转白色方块
- **下箭头键**：增大红色方块的尺寸，并在达到阈值时重置尺寸

项目目标如下：

- 理解异步加载的处理
- 构建支持未来资源加载与访问的基础设施
- 通过加载 GLSL 着色器文件来体验异步资源加载

> **注意**  
> 有关 JavaScript 异步操作的更多信息，你可以参考网上许多优秀的资源，例如 <https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous>。

## 向引擎添加资源映射组件

`resource_map` 引擎组件负责管理资源的加载、存储以及加载完成后的检索。这些操作属于游戏引擎内部，不应被游戏引擎客户端访问。与所有核心引擎组件（例如游戏循环）一样，其源代码文件创建在 `src/engine/core` 文件夹中。详情如下。

### 步骤 1：创建资源映射文件

在 `src/engine/core` 文件夹中创建一个新文件，命名为 `resource_map.js`。

### 步骤 2：定义 MapEntry 类

定义 `MapEntry` 类以支持已加载资源的引用计数。引用计数对于避免资源重复加载或过早卸载至关重要。

```javascript
class MapEntry {
    constructor(data) {
        this.mData = data;
        this.mRefCount = 1;
    }
    decRef() { this.mRefCount--; }
    incRef() { this.mRefCount++; }
    set(data) { this.mData = data; }
    data() { return this.mData; }
    canRemove() { return (this.mRefCount == 0); }
}
```

### 步骤 3：定义存储结构

定义一个键值对映射 `mMap`，用于资源的存储与检索；以及一个数组 `mOutstandingPromises`，用于捕获所有未完成的异步加载操作：

```javascript
let mMap = new Map();
let mOutstandingPromises = [];
```

> **注意**  
> JavaScript `Map` 对象保存键值对集合。

### 步骤 4：定义资源访问函数

定义用于查询存在性、检索和设置资源的函数。请注意，正如参数变量名 `path` 所暗示的，预期将外部资源文件的完整路径作为访问对应资源的键（例如，使用 `src/glsl_shaders/simple_vs.glsl` 文件的路径作为键来访问该文件的内容）。

```javascript
function has(path) { return mMap.has(path) }
function get(path) {
    if (!has(path)) {
        throw new Error("Error [" + path + "]: not loaded");
    }
    return mMap.get(path).data();
}
function set(key, value) { mMap.get(key).set(value); }
```

### 步骤 5：定义资源生命周期函数

定义函数以指示已请求加载、增加已加载资源的引用计数，并正确卸载资源。由于加载操作的异步性质，加载请求将导致一个空的 `MapEntry`，该条目将在未来某个时候加载操作完成时更新。请注意，每次卸载请求都会减少引用计数，但可能导致也可能不导致资源被卸载。

```javascript
function loadRequested(path) {
    mMap.set(path, new MapEntry(null));
}
function incRef(path) {
    mMap.get(path).incRef();
}
function unload(path) {
    let entry = mMap.get(path);
    entry.decRef();
    if (entry.canRemove())
        mMap.delete(path)
    return entry.canRemove();
}
```

### 步骤 6：定义 Promise 跟踪函数

定义一个函数，将正在进行的异步加载操作追加到 `mOutstandingPromises` 数组中：

```javascript
function pushPromise(p) { mOutstandingPromises.push(p); }
```

### 步骤 7：定义通用加载函数

定义一个加载函数 `loadDecodeParse()`。如果资源已加载，则增加相应的引用计数。否则，该函数首先发出 `loadRequest()` 以在 `mMap` 中创建一个空的 `MapEntry`。然后该函数创建一个 HTML5 `fetch` Promise，使用资源路径作为键，异步获取外部资源，解码网络封装，将结果解析为合适的格式，并将结果更新到创建的 `MapEntry` 中。然后将创建的 Promise 推入 `mOutstandingPromises` 数组。

```javascript
// 通用加载函数，
//  步骤 1：从服务器获取
//  步骤 2：对加载的包进行 decodeResource
//  步骤 3：对解码后的资源进行 parseResource
//  步骤 4：将结果存入映射
// 将承诺的操作推入数组
function loadDecodeParse(path, decodeResource, parseResource) {
    let fetchPromise = null;
    if (!has(path)) {
        loadRequested(path);
        fetchPromise = fetch(path)
            .then(res => decodeResource(res) )
            .then(data => parseResource(data) )
            .then(data => { return set(path, data) } )
            .catch(err => { throw err });
        pushPromise(fetchPromise);
    } else {
        incRef(path);  // 增加引用计数
    }
    return fetchPromise;
}
```

请注意，解码和解析函数作为参数传入，因此它们取决于正在获取的实际资源类型。例如，简单文本、XML（可扩展标记语言）格式文本、音频片段和图像的解码与解析都有各自不同的要求。定义这些函数是实际资源加载器的责任。

HTML5 `fetch()` 函数返回一个 JavaScript Promise 对象。一个典型的 JavaScript Promise 对象包含将在未来完成的操作。当操作完成时，Promise 即兑现。在本例中，当 `path` 被正确获取、解码、解析并更新到对应的 `MapEntry` 时，`fetchPromise` 即告兑现。该 Promise 被保存在 `mOutstandingPromises` 数组中。请注意，在 `loadDecodeParse()` 函数结束时，异步 `fetch()` 加载操作已发起并正在进行，但不保证已完成。这样，`mOutstandingPromises` 就是一个进行中且未兑现（即未完成）的 Promise 数组。

### 步骤 8：定义异步等待函数

定义一个 JavaScript `async` 函数，阻塞执行并等待所有未完成的 Promise 兑现，或等待所有正在进行的异步加载操作完成：

```javascript
// 将阻塞，等待所有未完成的 Promise 完成
// 然后继续
async function waitOnPromises() {
    await Promise.all(mOutstandingPromises);
    mOutstandingPromises = []; // 移除所有
}
```

> **注意**  
> JavaScript `async`/`await` 关键字成对使用，只有 `async` 函数才能 `await` 一个 Promise。`await` 语句会阻塞并将执行权返回给 `async` 函数的调用者。当被等待的 Promise 兑现时，执行将继续到 `async` 函数的末尾。

### 步骤 9：导出引擎功能

最后，向游戏引擎的其余部分导出功能：

```javascript
export {has, get, set,
    loadRequested, incRef, loadDecodeParse,
    unload,
    pushPromise, waitOnPromises}
```

