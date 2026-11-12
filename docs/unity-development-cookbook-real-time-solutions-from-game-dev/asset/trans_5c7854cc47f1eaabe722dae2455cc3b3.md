

向导是一种源远流长的用户界面范式，可以追溯到 20 世纪 90 年代初，它会引导你经历一系列步骤。当你在构建游戏时需要频繁执行某个重复性流程时，向导非常有用。

让我们创建一个向导，用于创建一个立方体，并创建一个使用指定颜色的新材质，这一切只需单击一个按钮即可完成：

- 1. 创建一个名为 CreateCubeWizard.cs 的新 C# 脚本，并向其中添加以下代码：

```
#if UNITY_EDITOR
// This entire class only exists in the editor. It doesn't need to be
// included in built games.
using UnityEditor;

// Create a wizard that generates a new cube, as well as a color.
public class CreateCubeWizard : ScriptableWizard {

// Create a new entry in the GameObject menu, called "Cube with
// Color." When it's selected, a CreateCubeWizard will appear. Note
// that this method must be both public and static for
// MenuItem to work.
[MenuItem("GameObject/Cube with Color")]
public static void CreateWizard() {
// Create and display the wizard.
DisplayWizard<CreateCubeWizard>("Create Cube");
}

// Stores temporary information about the cube that the user wants
// to make. These variables are drawn in the window, just like
// variables in a MonoBehaviour component are.
[SerializeField] Vector3 size = Vector3.zero;
[SerializeField] Color color = Color.white;

// Run when the Create button is clicked.
private void OnWizardCreate()
{
// Create a cube
var newCube = GameObject.CreatePrimitive(PrimitiveType.Cube);
// Scale it
newCube.transform.localScale = size;
// Create a new material, using the Standard shader (which
// is the default)
var tintedMaterial = new Material(Shader.Find("Standard"));
// Give it a color
tintedMaterial.color = color;
// Materials need to be saved to disk. To do this, we need to
// figure out where we can save the file.
// GenerateUniqueAssetPath will give us a path that's
// guaranteed to not already have a file present.
var desiredPath =
    AssetDatabase.GenerateUniqueAssetPath("Assets/Tinted.mat");
// Create and save the new asset.
AssetDatabase.CreateAsset(tintedMaterial, desiredPath);
AssetDatabase.SaveAssets();
// Visually "ping" the asset, as though we'd selected it in the
// Editor. This will show the user that a new file has been
// created, and where to find it.
EditorGUIUtility.PingObject(tintedMaterial);
}

// Finally, make the new cube use this new material.
newCube.GetComponent<MeshRenderer>().material = tintedMaterial;
}
}
#endif
```

我们定义了一个名为 CreateCubeWizard 的 ScriptableWizard，它添加了一个名为“Cube with Color”的新菜单项，让我们可以轻松触发向导，然后创建一个新立方体，要求用户提供用于 size 的 Vector3 和用于 color 的 Color。

- 2. 打开 GameObject 菜单，你会看到新的菜单项：Cube with Color。单击它。
- 3. 输入立方体的尺寸和颜色，然后单击 Create。
- 4. 新立方体将添加到场景中，将使用该颜色创建一个新材质，并将立方体设置为使用该新材质。

### 讨论

创建编辑器工具（例如向导）是更复杂的 Unity 工作流程的重要组成部分。向导是创建自定义 UI 的绝佳方式，可以让你作为游戏开发者的生活更轻松。

Unity Editor API 允许你构建许多有用的东西，来简化常见的工作流程。如果你发现自己反复执行某个动作多次，那么使用 Unity Editor API 来简化自己的工作通常是个好主意。自动化的可能性非常广泛！

> **提示**
> 
> 你可以在 Unity 文档中了解有关 Unity Editor API 的更多信息。有很多可能性，也有很多方法可以自动化重复的工作流程。

## 第 13 章。文件、网络和屏幕截图

丰富、复杂、可视化的计算机程序，比如你可能用 Unity 制作的那种，也需要完成平凡、传统的计算机工作，例如保存、加载和解析文件。我们把最精彩的章节留到了最后：在这里你会找到保存屏幕截图、状态和纹理的配方，以及使用自定义管道导入文件的配方。确实令人激动（其实不然），但确实必不可少（真的！）。

### 13.1 保存文件

### 问题

你想知道可以将项目生成的文件（如屏幕截图和已保存的游戏）保存到何处。

### 解决方案

使用 Application.persistentDataPath 属性来获取可用于保存数据的文件夹位置：

```
public string PathForFilename(string filename) {
    // Application.persistentDataPath contains a path where we can
    // safely store data
    var folderToStoreFilesIn = Application.persistentDataPath;

    // System.IO.Path.Combine combines two paths, using the current
    // system's directory separator ( \ on Windows, / on just about
    // every other platform)
    var path = System.IO.Path.Combine(folderToStoreFilesIn, filename);

    return path;
}
```

### 讨论

persistentDataPath 提供的目录不一定会向用户公开；例如，在用户无法直接访问文件系统的移动平台上，它不会公开。

### 13.2 将游戏图像文件保存到磁盘

### 问题

你想捕获游戏图像（屏幕截图），并将其保存到磁盘。

### 解决方案

你可以使用 ScreenCapture.CaptureScreenshot 方法来捕获屏幕截图：

```
// Capturing a screenshot at current resolution
ScreenCapture.CaptureScreenshot("MyScreenshot.png");
```

当你调用 CaptureScreenshot 时，Unity 会将生成的图像保存在 Application.persistentDataPath 指示的目录中。

屏幕截图的大小将与游戏正在渲染到的屏幕或窗口的大小相同。你也可以通过提供缩放因子作为参数来请求更大的屏幕截图。例如：

```
// Capturing a screenshot at double resolution (e.g., if the screen // resolution is 1920x1080, the resulting file will be 3840x2160)
ScreenCapture.CaptureScreenshot("MyScreenshotDoubleSized.png", 2);
```

### 讨论

当你调用 CaptureScreenshot 时，捕获不会立即发生。相反，Unity 会等待帧的末尾（当所有渲染都完成时）；此时，屏幕截图会被捕获并保存到磁盘。

> **提示**
> 
> 这意味着你每帧只能捕获一次屏幕截图。每次调用 CaptureScreenshot 实际上都是一次“请求”，稍后捕获屏幕截图，并且同一时间只会存储一个请求。

除了将屏幕截图保存为文件外，你还可以使用 CaptureScreenshotAsTexture 方法将其保存为纹理：

```
// Capturing a screenshot as a texture.
// Note that this captures the screenshot immediately, and it may not
// contain what you expect.
Texture2D capturedTexture =
ScreenCapture.CaptureScreenshotAsTexture();
```

与 CaptureScreenshot 不同，CaptureScreenshotAsTexture 会立即捕获屏幕截图，因为它需要返回捕获的纹理。这意味着纹理可能不包含你期望的最终图像。

要在帧末尾捕获，你需要一个协程，在捕获纹理之前等待帧的末尾，例如本示例：

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

你可以通过使用 StartCoroutine 调用该协程来运行它，如下所示：

```
// Begin the coroutine
StartCoroutine(CaptureScreenshotAtEndOfFrame());
```

### 13.3 从磁盘加载纹理

### 问题

你有一个存储在磁盘上的图像文件，并想在游戏中显示它。

### 解决方案

你可以从存储为 PNG 或 JPEG 图像的文件中加载任何纹理。在本示例中，我们将把一张图像放在项目文件夹（不是 Assets 文件夹，而是其上一级文件夹）中，名为 ImageToLoad.png：

- 1. 在你想用于执行加载的脚本中，将以下代码添加到文件顶部：

```
using System.IO;
```

System.IO 命名空间包含许多有用的类，用于处理磁盘上的文件。

- 2. 将以下方法添加到你要加载纹理文件的类中：

```
// Loads a texture file from disk
public Texture2D LoadTexture(string fileName) {
// Create the path to this file
// Application.dataPath is the path to where the built application
// is; in the editor, it is in the project's root folder (the one
```

