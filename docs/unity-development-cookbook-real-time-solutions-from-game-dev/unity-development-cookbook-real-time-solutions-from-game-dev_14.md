# 第 13 章 文件、网络和屏幕截图

丰富、复杂、可视化的计算机程序，比如你可能用 Unity 制作的那种，也需要完成平凡、传统的计算机工作，例如保存、加载和解析文件。我们把最精彩的章节留到了最后：在这里你会找到保存屏幕截图、状态和纹理的配方，以及使用自定义管道导入文件的配方。确实令人激动（其实不然），但确实必不可少（真的！）。

## 13.1 保存文件

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

## 13.2 将游戏图像文件保存到磁盘

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

## 13.3 从磁盘加载纹理

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

### 讨论

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





我们要保存的数据。将以下代码添加到 SavingService 类中：

```csharp
// To avoid problems caused by typos, we'll store the names of the
// strings we use to store and look up items in the JSON as
// constant strings
private const string ACTIVE_SCENE_KEY = "activeScene";
private const string SCENES_KEY = "scenes";
private const string OBJECTS_KEY = "objects";
private const string SAVEID_KEY = "$saveID";

// (use an unexpected character "$" for the Save ID here to reduce the
// chance of collisions)
```

- 4. 现在我们可以创建 SaveGame 方法，它执行实际保存场景的工作。将以下方法添加到 SavingService 类中：

```csharp
// Saves the game, and writes it to a file called fileName in the app's
// persistent data directory.
public static void SaveGame(string fileName) {
    // Create the JsonData that we will eventually write to disk
    var result = new JsonData();

    // Find all MonoBehaviours by first finding every MonoBehaviour,
    // and filtering it to only include those that are ISaveable.
    var allSaveableObjects = Object
        .FindObjectsOfType<MonoBehaviour>()
        .OfType<ISaveable>();

    // Do we have any objects to save?
    if (allSaveableObjects.Count() > 0) {
        // Create the JsonData that will store the list of objects
        var savedObjects = new JsonData();

        // Iterate over every object we want to save
        foreach (var saveableObject in allSaveableObjects)
        {
            // Get the object's saved data
            var data = saveableObject.SavedData;

            // We expect this to be an object (JSON's term for a
            // dictionary) because we need to include the object's Save
            // ID
            if (data.IsObject) {
                // Record the Save ID for this object
                data[SAVEID_KEY] = saveableObject.SaveID;

                // Add the object's save data to the collection
                savedObjects.Add(data);
            } else {
                // Provide a helpful warning that we can't save this
                // object.
                var behaviour = saveableObject as MonoBehaviour;

                Debug.LogWarningFormat(
                    behaviour,
                    "{0}'s save data is not a dictionary. The " +
                    "object was not saved.",
                    behaviour.name
                );
            }
        }

        // Store the collection of saved objects in the result.
        result[OBJECTS_KEY] = savedObjects;
    } else {
        // We have no objects to save. Give a nice warning.
        Debug.LogWarningFormat(
            "The scene did not include any saveable objects.");
    }

    // Next, we need to record what scenes are open. Unity lets you
    // have multiple scenes open at the same time, so we need to store
    // all of them, as well as which scene is the "active" scene (the
    // scene that new objects are added to, and that controls the
    // lighting settings for the game).

    // Create a JsonData that will store the list of open scenes.
    var openScenes = new JsonData();

    // Ask the scene manager how many scenes are open, and for each one,
    // store the scene's name.
    var sceneCount = SceneManager.sceneCount;

    for (int i = 0; i < sceneCount; i++) {
        var scene = SceneManager.GetSceneAt(i);

        openScenes.Add(scene.name);
    }

    // Store the list of open scenes
    result[SCENES_KEY] = openScenes;

    // Store the name of the active scene
    result[ACTIVE_SCENE_KEY] = SceneManager.GetActiveScene().name;

    // We've now finished generating the save data, and it's time to
    // write it to disk.

    // Figure out where to put the file by combining the persistent
    // data path with the filename that this method received as a
    // parameter.
    var outputPath = Path.Combine(
        Application.persistentDataPath, fileName);

    // Create a JsonWriter, and configure it to 'pretty-print' the
    // data. This is optional (you could just call result.ToJson() with
    // no JsonWriter parameter and receive a string), but this way the
    // resulting JSON is easier to read and understand, which is
    // helpful while developing.
    var writer = new JsonWriter();
    writer.PrettyPrint = true;

    // Convert the save data to JSON text.
    result.ToJson(writer);

    // Write the JSON text to disk.
    File.WriteAllText(outputPath, writer.ToString());

    // Notify where to find the saved game
    Debug.LogFormat("Wrote saved game to {0}", outputPath);

    // We allocated a lot of memory here, which means that there's an
    // increased chance of the garbage collector needing to run in the
    // future. To tidy up, we'll release our reference to the saved
    // data, and then ask the garbage collector to run immediately.
    // This will result in a slight performance hitch as the collector
    // runs, but that's fine for this case, since users expect saving
    // the game to pause for a second.
    result = null;

    System.GC.Collect();
}
```

SaveGame 通过首先创建一个集合来实现，该集合包含每个同时实现 ISaveable 接口的 MonoBehaviour 类。实现此接口的对象能够创建用于在 JSON 文档中存储的数据；对于找到的每个对象，我们获取该数据并将其存储在一个最终会被写入磁盘的 JsonData 对象中。

我们还会创建当前打开的场景列表并将其存储。我们还会包含当前活动场景的名称。

最后，我们将所有数据转换为包含文本 JSON 表示的字符串，确定在磁盘上的保存位置，并保存文件。

## 实现加载

既然我们知道了如何保存游戏状态，就需要实现加载数据的系统。

加载游戏就像保存游戏的逆过程：我们加载文件、解析 JSON、获取场景列表并加载它们、遍历保存的对象数据列表，并将加载的数据交给每个相应的对象。

在编写 LoadGame 方法之前，我们需要做一点工作来处理 Unity 加载场景时的一个特性。使用 SceneManager.LoadScene 加载场景时，你的代码会等待 Unity 完成场景加载。但是，场景在返回时尚未完全加载完成；如果你立即更改场景中的对象，这些更改会在下一帧被覆盖。为了绕过这个问题，我们会注册一些在场景真正加载完成后运行的代码。这段代码将存储在一个变量中，我们会在场景加载完成后将其传递给 SceneManager。

> **注意**
>
> 因为 SavingService 类是静态的，它拥有的任何变量也必须是静态的。是的，这是全局状态，通常被认为是应避免的事情，如果游戏中需要多个保存/加载系统，这种做法就行不通。但是，由于保存和加载等系统的工作方式是替换*游戏的整个状态*，因此拥有多个保存系统的想法并不真正适用。所以，在这里使用少量的全局状态是可以的。

### 实现加载：

- 1. 将以下变量添加到 SavingService 类中：

```csharp
// A reference to the delegate that runs after the scene loads, which
// performs the object state restoration.
static UnityEngine.Events.UnityAction<Scene, LoadSceneMode>
    LoadObjectsAfterSceneLoad;
```

- 2. 现在我们终于可以添加 LoadGame 方法了，它从文件加载游戏状态。将以下方法添加到 SavingService 类中：

```csharp
// Loads the game from a given file, and restores its state.
public static bool LoadGame(string fileName) {
    // Figure out where to find the file.
    var dataPath = Path.Combine(
        Application.persistentDataPath, fileName);

    // Ensure that a file actually exists there.
    if (File.Exists(dataPath) == false) {
        Debug.LogErrorFormat("No file exists at {0}", dataPath);
        return false;
    }

    // Read the data as JSON.
    var text = File.ReadAllText(dataPath);

    var data = JsonMapper.ToObject(text);

    // Ensure that we successfully read the data, and that it's an object
    // (i.e., a JSON dictionary)
    if (data == null || data.IsObject == false) {
        Debug.LogErrorFormat(
            "Data at {0} is not a JSON object", dataPath);
        return false;
    }

    // We need to know what scenes to load.
    if (!data.ContainsKey("scenes"))
    {
        Debug.LogWarningFormat(
            "Data at {0} does not contain any scenes; not " +
            "loading any!.",
            dataPath
        );
        return false;
    }

    // Get the list of scenes
    var scenes = data[SCENES_KEY];

    int sceneCount = scenes.Count;

    if (sceneCount == 0)
    {
        Debug.LogWarningFormat(
            "Data at {0} doesn't specify any scenes to load.",
            dataPath
        );
        return false;
    }

    // Load each specified scene.
    for (int i = 0; i < sceneCount; i++)
    {
        var scene = (string)scenes[i];

        // If this is the first scene we're loading, load it and replace
        // every other active scene.
        if (i == 0)
        {
            SceneManager.LoadScene(scene, LoadSceneMode.Single);
        }
        else
        {
            // Otherwise, load that scene on top of the existing ones.
            SceneManager.LoadScene(scene, LoadSceneMode.Additive);
        }
    }
}
```





```
}
```

```
// Find the active scene, and set it
if (data.ContainsKey(ACTIVE_SCENE_KEY))
{
    var activeSceneName = (string)data[ACTIVE_SCENE_KEY];
    var activeScene = SceneManager.GetSceneByName(activeSceneName);

    if (activeScene.IsValid() == false)
    {
        Debug.LogErrorFormat(
            "Data at {0} specifies an active scene that " +
            "doesn't exist. Stopping loading here.",
            dataPath
        );
        return false;
    }
}
```

```
SceneManager.SetActiveScene(activeScene);
```

```
} else {
    // This is not an error, since the first scene in the list will
    // be treated as active, but it's worth warning about.
    Debug.LogWarningFormat("Data at {0} does not specify an " +
        "active scene.", dataPath);
}
```

```
// Find all objects in the scene and load them
if (data.ContainsKey(OBJECTS_KEY)) {
    var objects = data[OBJECTS_KEY];
```

```
// We can't update the state of the objects right away because
// Unity will not complete loading the scene until sometime in
// the future. Changes we made to the objects would revert
// to how they're defined in the original scene. As a result, we
// need to run the code after the scene manager reports that a
// scene has finished loading.
```

```
// To do this, we create a new delegate that contains our object-
// loading code, and store that in LoadObjectsAfterSceneLoad.
// This delegate is added to the SceneManager's sceneLoaded
// event, which makes it run after the scene has finished loading.
```

```
LoadObjectsAfterSceneLoad = (scene, loadSceneMode) => {
```

```
// Find all ISaveable objects, and build a dictionary that maps
// their Save IDs to the object (so that we can quickly look
// them up)
```

```
var allLoadableObjects = Object
    .FindObjectsOfType<MonoBehaviour>()
    .OfType<ISaveable>()
    .ToDictionary(o => o.SaveID, o => o);

// Get the collection of objects we need to load
var objectsCount = objects.Count;

// For each item in the list...
for (int i = 0; i < objectsCount; i++)
{
    // Get the saved data
    var objectData = objects[i];

    // Get the Save ID from that data
    var saveID = (string)objectData[SAVEID_KEY];

    // Attempt to find the object in the scene(s) that has that
    // Save ID
    if (allLoadableObjects.ContainsKey(saveID))
    {
        var loadableObject = allLoadableObjects[saveID];

        // Ask the object to load from this data
        loadableObject.LoadFromData(objectData);
    }
}

// Tidy up after ourselves; remove this delegate from the
// sceneLoaded event so that it isn't called next time
SceneManager.sceneLoaded -= LoadObjectsAfterSceneLoad;
```

```
// Release the reference to the delegate
LoadObjectsAfterSceneLoad = null;
```

```
// And ask the garbage collector to tidy up (again, this will
// cause a performance hitch, but users are fine with this as
// they're already waiting for the scene to finish loading)
System.GC.Collect();
};
```

```
// Register the object-loading code to run after the scene loads.
SceneManager.sceneLoaded += LoadObjectsAfterSceneLoad;
}

return true;
}
```

LoadGame 方法读取文件，并首先加载保存的游戏中指定的场景。之后，它会创建并保存用于恢复场景中对象状态的代码，并将其添加到 SceneManager 的 sceneLoaded 事件中。该代码本身会查找所有 ISaveable MonoBehaviour，创建一个将其 Save ID 映射到对象本身的字典，然后使用加载的对象数据来查找适合该数据的对象，并将数据交付给该对象。

```
// Save the game to a file called "SaveGame.json"
SavingService.SaveGame("SaveGame.json");
```

要加载游戏，请调用 LoadGame 方法：

```
// Try to load the game from a file called "SaveGame.json"
SavingService.LoadGame("SaveGame.json");
```

这些方法可以从任何位置调用，但请注意，保存游戏不是一项轻量级操作。不要每帧都保存；最多每隔几分钟保存一次。

## 创建 SaveableBehaviour

保存系统依赖于对象具有一个唯一的 Save ID。这个 ID 理论上可以手动指定，但并无必要——它从不对用户公开，并且每个 ID 都必须唯一。此外，ID 在保存游戏和再次加载之间不能更改，这意味着它需要存储在对象本身内部。

因此，最好是有一个系统能为你自动处理此事，所以我们将亲手创建一个。我们将继承 MonoBehaviour 类，并添加代码，以便在必要时自动生成并公开 Save ID。我们将利用这样一个事实：Unity 允许你编写在 Unity 将对象序列化到场景中时运行的代码，在编辑器中处理对象时这种情况会频繁发生。

另外，我们将使这个子类成为抽象类，因此我们只实现 SaveID 属性，而将 SaveData 和 LoadFromData 方法留给子类。

创建一个名为 SaveableBehaviour.cs 的新 C# 脚本，并向其中添加以下代码：

```
public abstract class SaveableBehaviour : MonoBehaviour,
    ISaveable,
    ISerializationCallbackReceiver // Asks Unity to run code when the
    // scene file is saved in the editor
{
    // This class doesn't implement SavedData or LoadFromData; that's the
    // job of our subclasses.
    public abstract JsonData SavedData { get; }
    public abstract void LoadFromData(JsonData data);

    // This class does implement the SaveID property; it wraps the _saveID
    // field. (We need to do this manually, rather than using automatic
    // property generation (i.e., "public string SaveID {get;set;}"),
    // because Unity won't store automatic properties when saving the
    // scene file.

    public string SaveID
    {
        get
        {
            return _saveID;
        }
        set
        {
            _saveID = value;
        }
    }

    // The _saveID field stores the actual data that SaveID uses. We mark
    // it as serialized so that the Unity editor saves it with the rest of the
    // scene, and ask HideInInspector to make it not appear in the Inspector
    // (there's no reason for it to be edited).
    [HideInInspector]
    [SerializeField]
    private string _saveID;

    // OnBeforeSerialize is called when Unity is about to save this object
    // as part of a scene file.
```

当 SaveableBehaviour 类在编辑器中作为场景的一部分被保存时，会调用其 OnBeforeSerialize 方法。如果此时 SaveableBehaviour 没有 Save ID，它会生成一个并将其存储在一个 private 变量中（这样其他类就无法访问和修改 Save ID；它们也不需要知道它），并标记为 SerializeField（以便 Unity 将其存储在场景文件中），并设置为 HideInInspector（以便它不出现在 Inspector 中，因为没有理由对其进行编辑）。

> **注意**
> 
> 该类为抽象类，这意味着你无法直接创建 SaveableBehaviour 的实例。相反，你应当继承 SaveableBehaviour 并实现 SaveData 和 LoadFromData 方法。SaveID 属性会为你自动处理。

## 创建可保存的 MonoBehaviour

为了演示如何构建能够保存其数据的脚本，我们将构建一个示例 TransformSaver.cs，它存储附加到其上的游戏对象的位置、旋转和缩放信息：

- 1. 创建一个名为 TransformSaver.cs 的新 C# 脚本，并添加以下代码：

```
public class TransformSaver : SaveableBehaviour
{
    // Store the keys we'll be including in the saved game as constants,
    // to avoid problems with typos.
    private const string LOCAL_POSITION_KEY = "localPosition";
    private const string LOCAL_ROTATION_KEY = "localRotation";
    private const string LOCAL_SCALE_KEY = "localScale";

    // SerializeValue is a helper function that converts an object that
    // Unity already knows how to serialize (like Vector3, Quaternion,
    // and others) into a JsonData that can be included in the
    // saved game.
    private JsonData SerializeValue(object obj) {
        // This is very inefficient (we're converting an object to JSON
        // text, then immediately parsing this text back into a JSON
        // representation), but it means that we don't need to write the
        // (de)serialization code for built-in Unity types
        return JsonMapper.ToObject(JsonUtility.ToJson(obj));
    }

    // DeserializeValue works in reverse: given a JsonData, it produces
    // a value of the desired type, as long as that type is one that
    // Unity already knows how to serialize.
    private T DeserializeValue<T>(JsonData data) {
        return JsonUtility.FromJson<T>(data.ToJson());
    }
}
```



// Provides the saved data for this component.
public override JsonData SavedData {
    get {
        // Create the JsonData that we'll return to the saved
        // game system
        var result = new JsonData();

        // Store our position, rotation, and scale
        result[LOCAL_POSITION_KEY] =
            SerializeValue(transform.localPosition);

        result[LOCAL_ROTATION_KEY] =
            SerializeValue(transform.localRotation);

        result[LOCAL_SCALE_KEY] =
            SerializeValue(transform.localScale);

        return result;
    }
}

// Given some loaded data, updates the state of the component.
public override void LoadFromData(JsonData data)
{
    // We can't assume that the data will contain every piece of
    // data that we store; remember the programmer's adage,
    // "be strict in what you generate, and forgiving in what
    // you accept."

    // Accordingly, we test to see if each item exists in the
    // saved data

    // Update position
    if (data.ContainsKey(LOCAL_POSITION_KEY))
    {
        transform.localPosition =
        DeserializeValue<Vector3>(data[LOCAL_POSITION_KEY]);
    }

    // Update rotation
    if (data.ContainsKey(LOCAL_ROTATION_KEY))
    {
        transform.localRotation =
        DeserializeValue<Quaternion>(data[LOCAL_ROTATION_KEY]);
    }

    // Update scale
    if (data.ContainsKey(LOCAL_SCALE_KEY))
    {
        transform.localScale =
        DeserializeValue<Vector3>(data[LOCAL_SCALE_KEY]);
    }
}

2. 要使用此脚本，请将 TransformSaver 组件添加到游戏对象中。它将自动包含在已保存的游戏中，并在加载游戏时恢复其状态。

## 讨论

此系统不会自动恢复场景加载后创建的任何对象。如果要支持此功能，您需要在场景中有一个对象，该对象保留其创建的对象的引用，并在被要求提供保存数据时存储有关这些对象的其他信息。当对象被要求加载时，它会重新创建这些对象。

## 13.5 从 Web 下载和解析 JSON

### 问题

您希望能够从 Web 服务器下载 JSON 格式的数据，例如配置数据。

### 解决方案

解决这类问题有两个步骤：首先，您需要下载数据，其次，您需要解析数据。

> **注意**
> 本示例使用 LitJSON，即配方 13.4 中使用的同一 JSON 处理库。还有许多其他替代方案，例如 Json.NET，或者对于简单情况，可以使用 Unity 的内置 JSON 工具。

要从 Web 下载数据，请使用 UnityWebRequest 类创建请求，然后调用其 SendWebRequest 方法启动它：

```
// Create the request
UnityWebRequest request = UnityWebRequest.Get("https://httpbin.org/get");

// Send the request; we'll get back an operation that represents its
// work-in-progress state
UnityWebRequestAsyncOperation operation = request.SendWebRequest();
```

调用 SendWebRequest 时，它会开始与网络通信并立即返回一个 UnityWebRequestAsyncOperation。这是一个您可以用来获取请求进度信息的对象。

在大多数情况下，您会希望等待操作完成，然后（取决于它是否成功）采取某种操作。

您可以使用操作的 completed 事件来实现这一点：

```
// Tell the operation that we want it to run some code when it's
// finished:
operation.completed += (op) =>
{
    // The operation has finished. Now we'll figure out if it succeeded
    // or not.
    switch (request.result)
    {
    case UnityWebRequest.Result.Success:
        // It worked! Log the result so we can see the raw text it
        // downloaded.
        Debug.Log($"Success!");
        Debug.Log(request.downloadHandler.text);
        // You can also use downloadHandler.data to get the raw
        // bytes.
        break;
    case UnityWebRequest.Result.ConnectionError:
        // We failed to connect. The server address might not be
        // valid, or we might not have a working internet
        // connection.
        Debug.Log($"Connection error: {request.error}");
        break;
    case UnityWebRequest.Result.ProtocolError:
        // We connected, but the server didn't give us data. There
        // are many reasons why: file not found, permission denied,
        // internal server error...
        Debug.Log($"Protocol error: {request.error}");
        break;
    default:
        // Some other event happened. (Nothing good, or else the
        // status would have been Success.)
        Debug.Log($"Error: {request.result}");
        break;
    }
};
```

下载数据后，您可以使用 LitJSON 来解析 JSON 并从中获取数据。

在此示例中，我们从 URL 检索数据
https://httpbin.org/get。

> **注意**
>
> httpbin 是一个用于测试 HTTP 请求的有用网站，它会对多个不同的 URL 返回有用信息。您可以在 https://httpbin.org 上了解更多信息。

此 URL 返回的数据如下所示：

```
{
...
"headers": {
...
"host": "httpbin.org"
}
}
```

在此示例中，我们将获取 "host" 值的值，该值存储在 "headers" 值中：

```
private void ParseJson(string text)
{
// Given a string containing JSON, turn it into a JsonData.
JsonData data = JsonMapper.ToObject(text);

// Wrap all of our work in a try-catch so that we can correctly handle a
// situation where the data doesn't contain what we expect
try {

// Get the JsonData representing the "headers" object, and then get
// the string value for "host".
string host = (string)data["headers"]["Host"];

Debug.Log($"The host is {host}");
} catch (KeyNotFoundException) {
// We couldn't find either "headers" or "host" in the data. Log an
// error about it.
Debug.LogError($"Couldn't find the host in the downloaded data!");
}
}
```

## 讨论

您通过对 UnityWebRequest 调用 SendWebRequest 收到的 UnityWebRequestAsyncOperation 对象也可用于协程中：

```
UnityWebRequestAsyncOperation operation = request.SendWebRequest();
StartCoroutine(WaitForDownload(operation));
```

```
// ... elsewhere in your class:

// WaitForDownload is a coroutine that takes a web request operation,
// waits for it to finish, and then takes action based on the
// operation's result.
private IEnumerator WaitForDownload(UnityWebRequestAsyncOperation webOperation)
{
// In a coroutine, you can yield on the operation (because it's an
// AsyncOperation). The coroutine will wait until the operation
// completes, and then resume.
yield return webOperation;

// At this point, the operation is now complete,
// and we can work with the request
var request = webOperation.webRequest;

if (request.result == UnityWebRequest.Result.Success) {
var data = request.downloadHandler.text;
} else {
// Deal with the error
}
}
```

LitJSON 的 JsonMapper.ToObject 方法将 JSON 文本解析为 JsonData 对象，您可以像访问字典或数组一样访问该对象（具体取决于您解析的 JSON）。您可以在 LitJSON 的官方网站上找到其文档和 API。

## 13.6 脚本导入器

### 问题

您希望编写代码，让 Unity 能够导入一种新类型的文件。

### 解决方案

游戏开发并非全部围绕相同的文件类型展开。有时您需要创建自定义文件扩展名。Unity 可以帮您解决！

在此示例中，我们将创建一个自定义导入器，允许 Unity 识别以 .cube 结尾的文本文件，这些文件包含描述立方体大小和颜色的 JSON 数据。

首先，让我们创建一个资产，然后再为其编写导入器：

- 1. 打开您喜欢的文本编辑器，创建一个新的空文件。（如果您没有喜欢的编辑器，请打开 Visual Studio，并选择“新建” → “文件”。创建一个新的空文件。）

- 2. 将以下代码放入文件中：

```
{
    "size": {"x":1, "y":1, "z":2},
    "color": {"r":0.5, "g":1, "b":0.5, "a":1}
}
```

- 3. 将文件保存为 Unity 项目 Assets 文件夹中的 Test.cube。

- 4. 转到 Unity。请注意，虽然您的 Test.cube 文件在 Assets 文件夹中可见，但您无法在项目中使用它，因为 Unity 不知道如何处理 .cube 文件！让我们来修复这个问题。

- 5. 创建一个名为 CubeImporter.cs 的新 C# 脚本，并用以下代码替换其内容：

```
#if UNITY_EDITOR
using UnityEditor;

// At the time of writing, asset importers are in the Experimental
// module. By the time you're reading this, the API may have changed, so
// check the documentation.
```





```csharp
using UnityEditor.Experimental.AssetImporters;
```

```csharp
// A CubeDescription contains the variables that define our cubes. We'll
// create them by loading them from text files that contain JSON.
public struct CubeDescription {
  public Vector3 size;

  // storing the r, g, b, a values in this 4-component vector
  public Color color;
}
```

```csharp
// Indicate to Unity that this script imports files with the file
// extension ".cube," and that this is version 0 of the importer
// (changing the number makes Unity reimport assets of this type).
[ScriptedImporter(0, "cube")]
public class CubeImporter : ScriptedImporter {
```

```csharp
// Called by Unity to perform an import
public override void OnImportAsset(AssetImportContext ctx)
{
```

```csharp
// "ctx" contains information about the import that Unity wants
// us to do; it contains the path to the file, and we'll put
// the Unity objects into it when we're done
```

```csharp
// "cube" files will contain JSON that describes the color and
// size of the cube.
```

```csharp
// Create a variable to load the cube description into
CubeDescription cubeDescription;
```

```csharp
// Attempt to load the JSON.
try {
  var text = System.IO.File.ReadAllText(ctx.assetPath);
cubeDescription =
    JsonUtility.FromJson<CubeDescription>(text);
} catch (System.ArgumentException e) {
    // We failed to load the JSON. Maybe it's not valid. Report
    // the error.
    Debug.LogErrorFormat(
        "{0} is not a valid cube: {1}",
        ctx.assetPath, e.Message);
    return;
}

} catch (System.Exception e) {
    // We caught some other kind of exception, and can't
    // continue. Rethrow the error.
    throw e;
}

// Create a generic cube object, which we'll make changes to and
// save as a new asset.
var cubeObject = GameObject.CreatePrimitive(PrimitiveType.Cube);

// Get the last part of the filepath, and use it as the cube's
// name
string name =
    System.IO.Path.GetFileNameWithoutExtension(ctx.assetPath);

// Next, we'll create a cube that's the right size. The default
// cube mesh is 1x1x1; we'll scale it based on the size that was
// passed in.

// Copy the default cube mesh.
var cubeMesh =
    Instantiate(cubeObject.GetComponent<MeshFilter>()
        .sharedMesh);
```

```csharp
// Create a matrix that scales vertices by the given X, Y, and Z
// amounts.
var scaleMatrix = Matrix4x4.Scale(cubeDescription.size);
```

```csharp
// Get a copy of the vertices in the mesh.
var vertices = cubeMesh.vertices;
```

```csharp
// For each of these vertices, apply the scale by multiplying
// the matrix against the vertex.
for (int v = 0; v < vertices.Length; v++) {
    vertices[v] = scaleMatrix.MultiplyPoint(vertices[v]);
}
```

```csharp
// Store these scaled vertices in the mesh.
cubeMesh.vertices = vertices;
```

```csharp
// Tell the cube's MeshFilter to use this new mesh.
cubeObject.GetComponent<MeshFilter>().sharedMesh = cubeMesh;
```

```csharp
// Give the mesh a name.
cubeMesh.name = name + " Mesh";
```

```csharp
// Create a new material, using the Standard shader (which is
// the default)
var cubeMaterial = new Material(Shader.Find("Standard"));
```

```csharp
// Apply the color that we loaded.
cubeMaterial.color = cubeDescription.color;
```

```csharp
// Give it a name, too.
cubeMaterial.name = name + " Material";
```

```csharp
// Tell the cube's MeshRenderer to use this material.
cubeObject.GetComponent<MeshRenderer>().material = cubeMaterial;

// Now we store the objects we just created as assets.

// First, store the GameObject (the collection of components
// that uses and renders the mesh and material), and mark it as
// the "main" object.
ctx.AddObjectToAsset(name, cubeObject);
ctx.SetMainObject(cubeObject);

// We also need to store the mesh and material.
ctx.AddObjectToAsset(cubeMaterial.name, cubeMaterial);
ctx.AddObjectToAsset(cubeMesh.name, cubeMesh);
}
}
#endif
```

这看起来又长又吓人，但它只是一长串小步骤。它定义了一个名为 CubeDescription 的结构体，用来描述立方体是什么：一个 Vector3 和一个 Color。然后它创建了一个名为 CubeImporter 的 ScriptedImporter，读取文件中的 JSON，并使用文件中的立方体信息创建一个新网格。真神奇！

> **提示**
>
> 要了解有关 Unity 中 Vector3 和其他数学类型的更多信息，请参见第 4 章。

- 6. 返回 Unity。您的 Test.cube 文件现在将被导入。它现在包含三个 Unity 可用的资源：一个网格、一个材质和一个使用该网格和材质的游戏对象。您可以将文件拖入场景，就像其他任何模型一样。

## 讨论

使 Unity 能够理解您的自定义文件类型，将打开一个可能性的世界。例如，如果您构建了一个导出关卡文件的外部关卡编辑器，此系统将使您能够赋予 Unity 编辑器读取您的文件的能力。

> **提示**
>
> Unity 社区已为许多 Unity 原生不一定支持的常见文件格式构建了导入器。如果您缺少某个文件格式功能，请在尝试自行实现之前先在互联网上搜索！
