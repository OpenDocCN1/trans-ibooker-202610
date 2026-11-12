

```
// that contains the Assets folder)
var imagePath = Path.Combine(
    Application.dataPath,
    fileName
);

// Double-check that a file exists
if (File.Exists(imagePath) == false) {
    // Warn that there's no file there and give up
    Debug.LogWarningFormat("No file exists at path {0}",
        imagePath);
    return null;
}

// Load the file data.
var fileData = File.ReadAllBytes(imagePath);

// Create a new texture. When you create any texture, you specify
// the width and height; however, when you load a texture, it will
// automatically resize itself
var tex = new Texture2D(2, 2);

// Upload the image data. Unity will decompress it from PNG into raw
// pixels.
var success = tex.LoadImage(fileData);

if (success) {
    // We now have a texture!
    return tex;
} else {
    // Warn that we can't read this file
    Debug.LogWarningFormat("Failed to load texture at path {0}",
        imagePath);
    return null;
}
}
```

你现在可以使用此方法从磁盘加载纹理。一旦你拥有纹理，你可以随意使用它；例如，下面展示了如何使用 LoadTexture 方法获取纹理，然后将其赋予渲染器的材质：

```
// Load the texture from disk
var tex = LoadTexture("ImageToLoad.png");

// Check that we got it before we try to use it
if (tex == null) {
    return;
}

// We can now use this texture just like any other!

// For example, here's how we can set the main texture of the renderer
// that this script is on the same object as
GetComponent<Renderer>().material.mainTexture = tex;
```

## 讨论

通常，你在游戏中使用的纹理都是资源，你可以直接通过拖放到检查器的插槽中来使用它们。仅在构建时没有纹理且需要从其他位置加载纹理的情况下，才使用这种方法加载纹理。从磁盘加载和解压缩图像文件所需的时间比使用直接内置到游戏中的资源要长。

## 13.4 保存和加载游戏状态

### 问题

你希望能够将项目的状态保存到文件中，并在稍后加载该状态。

### 解决方案

保存和加载是那些看起来比实际实现更简单的过程之一。实际上，它是一件复杂的事情，你如何实现保存和加载的具体细节取决于你的项目需求。你需要存储多少数据？你会保存数据的频率是多少？你是需要保存游戏的精细状态，还是仅存储简单的信息（例如玩家已经到达的关卡）就足够了？

### 保存系统概述

在这个教程中，我们将介绍并讨论一种方法，它允许某些 MonoBehaviour 将其状态保存和加载到 JSON 文件中。它并不保存场景的 *完整* 状态；相反，它允许对象选择需要保存哪些数据，以及如何处理已加载的数据。

我们将使用 JSON 进行所有保存和加载操作，原因有二：

- JSON 是一种文本格式，这使得查看生成的结果和诊断问题变得更容易一些。
- JSON 不依赖固定架构，这意味着让事情运行起来需要稍微少一点工作。

这个保存和加载系统的总体策略如下：

- 我们将定义一个名为 ISaveable 的 C# 接口；能够保存其状态的 MonoBehaviour 将实现此接口。
- 保存意味着查找所有实现此接口的 MonoBehaviour，请求它们的保存数据，并使用这些数据创建保存文件。
- 加载意味着加载保存文件，使用其 ID 查找每个实现 ISaveable 的 MonoBehaviour，并向该对象提供用于恢复其状态的信息。

每个 ISaveable MonoBehaviour 在被要求提供其保存状态时提供的内容，以及在接收到保存状态时执行的操作，均由其自行决定。

要了解保存系统的工作原理，查看保存文件的样子会很有帮助。下面是一个示例；我们添加了注释来解释每个部分：

```
{
 // "objects" is a list of objects that stores the data of each saved
 // ISaveable. This saved game contains only a single saved object,
 // but you can have as many as you like.
"objects" : [
{
// Each object has a unique Save ID, which is used to find
// the object later when the game is loaded.
"$saveID": "805133b3-fad9-41ed-a253-fce07e3c4672",
// The data inside each saved object is determined by the
// object itself.
"localPosition": {
"x" : -1.97000002861023,
"y" : 0.0,
"z" : 0.0
},
"localRotation": {
"x" : 0.0,
"y" : 0.0,
"z" : 0.0,
"w" : 1.0
},
"localScale": {
"x" : 1.0,
"y" : 1.0,
"z" : 1.0
}
}
],
// "scenes" contains the list of scenes open when the game was saved.
"scenes": [
"SavingLoadingGameState",
"SavingDemoExtraScene"
],
// "activeScene" is the name of the currently active scene. The
// active scene is the one into which new objects are instantiated,
// and controls the lighting settings for the game.
"activeScene": "SavingLoadingGameState"
}
```

### 下载 LitJSON

要读写 JSON，我们将使用一个名为 LitJSON 的第三方开源库。

**注意**

Unity 提供了一个用于处理 JSON 的工具——JsonUtility 类——但它不满足我们保存和加载的需求，因为它基于 Unity 现有的序列化工具。这并不意味着它完全没用——事实上，我们将它用作此序列化系统的一部分以节省一些工作——但它不是一个通用的 JSON 库。

要安装 LitJSON，请执行以下步骤：

1. 在 Assets 目录中创建一个名为 LitJSON 的新文件夹。
2. 在浏览器中，转到 LitJSON 最新版本的 GitHub 页面。
3. 将源代码下载为 ZIP 文件。
4. 解压 ZIP 文件。它将包含一个名为 src 的文件夹。
5. 将 src 目录的内容复制到你创建的 LitJSON 文件夹中。

### 实现保存服务

我们现在准备好开始编写实现保存系统的代码了。这将作为一个 `static` 类实现，其中包含用于保存和加载场景的方法：

1. 创建一个名为 `SavingService.cs` 的新 C# 脚本。
2. 打开文件，删除 `SavingService` 类——我们将用自己的类替换它。
3. 将以下代码添加到文件顶部：

```
using LitJson;
using System.IO;
using System.Linq;
using UnityEngine.SceneManagement;
```

我们在此引入了一些命名空间，用于我们的保存系统：

- LitJson
  用于读写 JSON 数据

- System.IO
  用于处理文件输入和输出

- System.Linq
  用于简化查找场景中所有可保存脚本的任务

- UnityEngine.SceneManagement
  用于处理加载新场景（因为加载游戏意味着加载新场景）

> **注意**
> 使用 LINQ 会分配内存，这意味着垃圾回收器将在将来的某个时刻运行。这会导致卡顿，通常应予以避免。但是，保存游戏的情况相对较少，而且预计这本身就是一个中等沉重的操作（也就是说，玩家预期游戏会短暂暂停）。此外，在完成操作时调用 `System.GC.Collect`，我们会完成大部分原本会在将来某个不可预测的时刻进行的清理工作。在游戏过程中，请尽量避免使用 LINQ。

接下来，我们将定义 ISaveable 接口。实现此接口的 MonoBehaviour 将包含在保存的游戏中：

1. 将以下代码添加到 `SavingService.cs`：

```
// Any MonoBehaviour that implements the ISaveable interface will be
// saved in the scene, and loaded back
public interface ISaveable {
// The Save ID is a unique string that identifies a component in the
// save data. It's used for finding that object again when the game
// is loaded.
string SaveID { get; }

// The SavedData is the content that will be written to disk. It's
// asked for when the game is saved.
JsonData SavedData { get; }

// LoadFromData is called when the game is being loaded. The object
// is provided with the data that was read, and is expected to use
// that information to restore its previous state.
void LoadFromData(JsonData data);
}
```

2. 现在我们可以开始构建 SavingService 类了。将以下代码添加到 SavingService.cs：

```
public static class SavingService
{

}
```

> ### 提示
> 
> 静态类是指其所有成员都必须是静态的类；也就是说，它们属于类本身，而不属于类的任何实例。静态类不能被实例化。实际上，我们将此类用作方法的容器，而不是用来创建实例（如 MonoBehaviour）。

3. 我们的下一步是定义一些将出现在保存游戏文件中的重要字符串。JSON 对象的工作方式类似于使用字符串作为键的字典；我们将使用这些键来引用保存文件的不同部分。

