

## 文件、网络和截图

像 Unity 这样的丰富、复杂、视觉化的计算机程序，也需要执行一些平凡、传统的计算机操作，比如保存、加载和解析文件。我们把最激动人心的章节留到了最后：在这里你会找到保存截图、状态和纹理的配方，以及使用自定义管线导入文件的配方。极其激动人心（其实不然），但极其必要（真的！）。

## 13.1 保存文件

### 问题

你想知道可以把项目生成的文件，比如截图和存档，保存到哪里。

### 解决方案

使用 `Application.persistentDataPath` 属性来获取可以保存数据的文件夹位置：

```
public string PathForFilename(string filename) {

    // Application.persistentDataPath contains a path where we can
    // safely store data
    var folderToStoreFilesIn = Application.persistentDataPath;

    // System.IO.Path.Combine combines two paths, using the current
    // system's directory separator ( \ on Windows, / on just about
    // every other platform)
    var path = System.IO.Path.Combine(folderToStoreFilesIn, filename);
```

```
return path;
}
```

### 讨论

`persistentDataPath` 提供的目录不保证对用户可见；例如，在用户无法直接访问文件系统的移动平台上，它将不会被暴露。

## 13.2 将游戏的图像文件保存到磁盘

### 问题

你想捕获游戏的图像（截图），并将其保存到磁盘。

### 解决方案

你可以使用 `ScreenCapture.CaptureScreenshot` 方法来捕获截图：

```
// Capturing a screenshot at current resolution
ScreenCapture.CaptureScreenshot("MyScreenshot.png");
```

调用 `CaptureScreenshot` 时，Unity 会将生成的图像保存在 `Application.persistentDataPath` 指示的目录中。

截图的大小与游戏渲染到的屏幕或窗口大小相同。你也可以通过提供缩放因子参数来请求更大的截图。例如：

```
// Capturing a screenshot at double resolution (e.g., if the screen
// resolution is 1920x1080, the resulting file will be 3840x2160)
ScreenCapture.CaptureScreenshot("MyScreenshotDoubleSized.png", 2);
```

### 讨论

调用 `CaptureScreenshot` 时，捕获不会立即发生。相反，Unity 会等待帧结束，待所有渲染完成后；此时截图会被捕获并保存到磁盘。

![](img/e54c00769db669e3746fa1e50090241f_388_0.png)

这意味着你每帧只能捕获一张截图。对 `CaptureScreenshot` 的每次调用实际上都是“稍后捕获截图”的请求，且一次只能存储一个请求。

除了将截图保存为文件外，你还可以使用 `CaptureScreenshotAsTexture` 方法将其保存为纹理：

```
// Capturing a screenshot as a texture.
// Note that this captures the screenshot immediately, and it may not
// contain what you expect.
Texture2D capturedTexture =
    ScreenCapture.CaptureScreenshotAsTexture();
```

与 `CaptureScreenshot` 不同，`CaptureScreenshotAsTexture` 会立即捕获截图，因为它需要返回捕获的纹理。这意味着纹理可能不包含你期望的最终图像。

要使其在帧结束时捕获，你需要一个在捕获纹理前等待帧结束的协程，例如以下示例：

```
IEnumerator CaptureScreenshotAtEndOfFrame() {

    // Wait until the very end of the frame, after all rendering has
    // completed
    yield return new WaitForEndOfFrame();

    // Capture the screenshot
    Texture2D capturedTexture =
        ScreenCapture.CaptureScreenshotAsTexture();

    // capturedTexture now contains the contents of the screen, after
    // everything in the frame has rendered
}
```

你可以通过调用 `StartCoroutine` 来运行此协程，如下所示：

```
// Begin the coroutine
StartCoroutine(CaptureScreenshotAtEndOfFrame());
```

## 13.3 从磁盘加载纹理

### 问题

你有一个存储在磁盘上的图像文件，想在游戏中显示它。

### 解决方案

你可以从存储在 PNG 或 JPEG 图像中的任何文件加载纹理。在此示例中，我们将在项目文件夹（不是 Assets 文件夹，其上层文件夹）中放置一个名为 `ImageToLoad.png` 的图像：

- 1. 在你想用来执行加载的脚本中，在文件顶部添加以下代码：

```
using System.IO;
```

`System.IO` 命名空间包含许多用于在磁盘上处理文件的有用类。

- 2. 向你想加载纹理文件的类中添加以下方法：

```
// Loads a texture file from disk
public Texture2D LoadTexture(string fileName) {
    // Create the path to this file

    // Application.dataPath is the path to where the built application
    // is; in the editor, it is in the project's root folder (the one
    // that contains the Assets folder)
    var imagePath = Path.Combine(
        Application.dataPath,
        fileName
    );

    // Double-check that a file exists
    if (File.Exists(imagePath) == false) {
        // Warn that there's no file there and give up
        Debug.LogWarningFormat("No file exists at path {0}", imagePath);

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
        Debug.LogWarningFormat("Failed to load texture at path {0}", imagePath);

        return null;
    }
}
```

现在你可以使用此方法从磁盘加载纹理。一旦获得纹理，你可以随意使用它；例如，这里是如何使用 `LoadTexture` 方法获取纹理然后将其赋给渲染器的材质：

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

### 讨论

通常，你在游戏中使用的纹理是资源，你通过将其拖放到 Inspector 中的槽位来直接使用。仅在你构建时没有纹理，需要从其他地方加载时，才使用这种加载纹理的方法。从磁盘加载和解压缩图像文件所需的时间，比使用直接构建到游戏中的资源要长。

## 13.4 保存和加载游戏状态

### 问题

你希望能够将项目状态保存到文件中，并在以后加载该状态。

### 解决方案

保存和加载听起来实现起来很简单，但实际上很复杂。保存和加载的具体实现方式取决于项目的需要。你需要存储多少数据？你会多久保存一次数据？你需要保存游戏的精细状态，还是存储简单信息，比如玩家到达的关卡就足够了？

### 保存系统的概述

在本配方中，我们将介绍并讨论一种方法，该方法允许某些 MonoBehaviour 将其状态保存并加载到 JSON 文件中。它不会保存整个场景的状态；相反，它允许对象选择需要保存的数据，以及加载数据后要执行的操作。

我们将使用 JSON 进行所有保存和加载，原因有两个：

- JSON 是文本格式，这使得查看生成的結果并诊断问题变得更容易一些。
- JSON 不依赖于固定模式，这意味着让事情正常工作所需的工作量少一些。

此保存和加载系统的总体策略如下：

- 我们将定义一个名为 `ISaveable` 的 C# 接口；可以保存其状态的 MonoBehaviour 实现此接口。
- 保存意味着找到所有实现此接口的 MonoBehaviour，请求其保存数据，并使用该数据创建保存文件。
- 加载意味着加载保存文件，使用其 ID 查找每个实现 `ISaveable` 的 MonoBehaviour，并为该对象提供用于恢复的信息。

每个 `ISaveable` MonoBehaviour 在被要求提供其保存状态时提供什么，以及在接收到保存状态时做什么，由它自己决定。

要了解保存系统的工作原理，查看保存文件是什么样子的会有所帮助。以下是一个示例；我们添加了注释来解释每个部分：

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
```

