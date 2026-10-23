## 文件、网络和截图

像 Unity 这样的丰富、复杂、视觉化的计算机程序，也需要执行一些平凡、传统的计算机操作，比如保存、加载和解析文件。我们把最激动人心的章节留到了最后：在这里你会找到保存截图、状态和纹理的配方，以及使用自定义管线导入文件的配方。极其激动人心（其实不然），但极其必要（真的！）。

### 13.1 保存文件

#### 问题

你想知道可以把项目生成的文件，比如截图和存档，保存到哪里。

#### 解决方案

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

#### 讨论

`persistentDataPath` 提供的目录不保证对用户可见；例如，在用户无法直接访问文件系统的移动平台上，它将不会被暴露。

### 13.2 将游戏的图像文件保存到磁盘

#### 问题

你想捕获游戏的图像（截图），并将其保存到磁盘。

#### 解决方案

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

#### 讨论

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

### 13.3 从磁盘加载纹理

#### 问题

你有一个存储在磁盘上的图像文件，想在游戏中显示它。

#### 解决方案

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

#### 讨论

通常，你在游戏中使用的纹理是资源，你通过将其拖放到 Inspector 中的槽位来直接使用。仅在你构建时没有纹理，需要从其他地方加载时，才使用这种加载纹理的方法。从磁盘加载和解压缩图像文件所需的时间，比使用直接构建到游戏中的资源要长。

### 13.4 保存和加载游戏状态

#### 问题

你希望能够将项目状态保存到文件中，并在以后加载该状态。

#### 解决方案

保存和加载听起来实现起来很简单，但实际上很复杂。保存和加载的具体实现方式取决于项目的需要。你需要存储多少数据？你会多久保存一次数据？你需要保存游戏的精细状态，还是存储简单信息，比如玩家到达的关卡就足够了？

#### 保存系统的概述

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





```
"z": 0.0,
"w": 1.0
},
"localScale": {
"x": 1.0,
"y": 1.0,
"z": 1.0
}
},
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

#### Downloading LitJSON

要读取和写入我们的 JSON，我们将使用一个名为 LitJSON 的第三方开源库。

![](img/e54c00769db669e3746fa1e50090241f_393_0.png)

Unity 提供了一个用于处理 JSON 的工具——JsonUtility 类，但它无法满足我们的保存和加载需求，因为它建立在 Unity 现有的序列化工具之上。这并不意味着它完全没用——事实上，我们在本次序列化系统中也使用了它来节省一些工作——但它并不是一个通用用途的 JSON 库。

要安装 LitJSON，请按照以下步骤操作：

- 1. 在你的 Assets 目录中新建一个名为 LitJSON 的文件夹。
- 2. 在浏览器中，前往 LitJSON 最新发布的 GitHub 页面。
- 3. 下载源代码作为 ZIP 文件。
- 4. 解压 ZIP 文件。它将包含一个名为 src 的文件夹。
- 5. 将 src 目录的内容复制到你创建的 LitJSON 文件夹中。

#### Implementing the saving service

我们现在准备开始编写实现保存系统的代码。这将实现为一个静态类，其中包含保存和加载场景的方法：

- 1. 创建一个新的 C# 脚本，命名为 SavingService.cs。
- 2. 打开文件，并删除 SavingService 类——我们将用自己的类替换它。
- 3. 在文件顶部添加以下代码：

```
using LitJson;
using System.IO;
using System.Linq;
using UnityEngine.SceneManagement;
```

我们在保存系统中引入了多个命名空间：

LitJson
    用于读取和写入 JSON 数据

System.IO
    用于处理文件输入和输出

System.Linq
    用于简化在场景中查找所有可保存脚本的任务

UnityEngine.SceneManagement
    用于处理加载新场景（因为加载游戏意味着加载新场景）

![](img/e54c00769db669e3746fa1e50090241f_394_0.png)

使用 LINQ 会分配内存，这意味着垃圾回收器将来某个时候必须运行。这会导致卡顿，通常应避免。然而，保存游戏发生的相对较少，而且本身就预期是一项中等重量的操作（也就是说，玩家期望游戏短暂暂停）。此外，在完成后调用 System.GC.Collect，我们完成了本应在未来某个不可预测的时间发生的清理工作。尽量避免在游戏过程中使用 LINQ。

接下来，我们将定义 ISaveable 接口。实现此接口的 MonoBehaviours 将包含在已保存的游戏中：

- 1. 在 SavingService.cs 中添加以下代码：

```
// Any MonoBehaviour that implements the ISaveable interface will be
// saved in the scene, and loaded back
```

```
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

2. 我们现在可以开始构建 SavingService 类。在 SavingService.cs 中添加以下代码：

```
public static class SavingService
{ 
}
```

![](img/e54c00769db669e3746fa1e50090241f_395_0.png)

静态类是指其所有成员都必须是静态的；也就是说，它们属于类本身，而不是类的任何实例。静态类不能被实例化。实际上，我们将此类的用作方法的容器，而不是像 MonoBehaviour 那样创建实例的对象。

3. 下一步是定义一些将在保存游戏文件中出现的字符串。JSON 对象就像使用字符串作为键的字典；我们将使用这些键来引用我们保存的数据的不同部分。将以下代码添加到 SavingService 类中：

```
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

4. 我们现在可以创建 SaveGame 方法，该方法执行实际保存场景的工作。将以下方法添加到 SavingService 类中：

```
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
    }
```

```
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
```





```
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

SaveGame 的工作原理是首先创建所有同时实现了 MonoBehaviour 类和 ISaveable 接口的类的集合。实现此接口的对象能够为存储在 JSON 文档中的数据创建数据；对于我们找到的每个对象，我们获取该数据并将其存储在最终将写入磁盘的 JsonData 对象中。

我们还创建了当前打开的场景列表并将其存储起来。我们还包括当前活动场景的名称。

最后，我们将所有数据转换为包含文本 JSON 表示的字符串，确定在磁盘上保存它的位置，然后保存文件。

#### 实现加载

现在我们知道了如何保存游戏状态，我们需要实现加载数据的系统。

加载游戏的工作方式与保存游戏相反：我们加载文件，解析 JSON，获取场景列表，加载它们，遍历保存的对象数据列表，并将加载的数据传递给每个适当的对象。

在编写 LoadGame 方法之前，我们需要做一点工作来处理 Unity 加载场景时的一个特殊情况。当你使用 Scene Manager.LoadScene 加载场景时，你的代码会等待 Unity 完成场景加载。但是，场景在返回时并没有完全完成加载；如果你立即对场景中的对象进行更改，它们将在下一帧被覆盖。为了解决这个问题，我们将注册一些在场景*真正*完成加载后运行的代码。此代码将存储在一个变量中，我们将在 SceneManager 加载场景后将其传递给它。

![](img/e54c00769db669e3746fa1e50090241f_399_0.png)

由于 SavingService 类是静态的，它拥有的任何变量也必须是静态的。是的，这是全局状态，通常被认为是要避免的东西，如果我们需要在游戏中拥有多个保存/加载系统，这将无法工作。但由于保存和加载等系统的工作方式是通过替换*整个游戏状态*，拥有多个保存系统的想法实际上并不适用。因此，在这里使用一点全局状态是可以的。

要实现加载：
- 1. 向 SavingService 类添加以下变量：
- 
      // A reference to the delegate that runs after the scene loads, which
      // performs the object state restoration.
      static UnityEngine.Events.UnityAction<Scene, LoadSceneMode>
          LoadObjectsAfterSceneLoad;
- 2. 我们现在终于可以添加 LoadGame 方法，该方法从文件中加载游戏状态并恢复其状态。将以下方法添加到 SavingService 类：

```
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
}
```

```
// We need to know what scenes to load.
if (!data.ContainsKey("scenes"))
{
    Debug.LogWarningFormat(
        "Data at {0} does not contain any scenes; not " +
        "loading any!",
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

// Find the active scene, and set it
if (data.ContainsKey(ACTIVE_SCENE_KEY))
{
    var activeSceneName = (string)data[ACTIVE_SCENE_KEY];
    var activeScene = SceneManager.GetSceneByName(activeSceneName);

    if (activeScene.IsValid() == false)
```

```
{
    Debug.LogErrorFormat(
        "Data at {0} specifies an active scene that " +
        "doesn't exist. Stopping loading here.",
        dataPath
    );
    return false;
}

SceneManager.SetActiveScene(activeScene);
} else {
    // This is not an error, since the first scene in the list will
    // be treated as active, but it's worth warning about.
    Debug.LogWarningFormat("Data at {0} does not specify an " +
        "active scene.", dataPath);
}

// Find all objects in the scene and load them
if (data.ContainsKey(OBJECTS_KEY)) {
    var objects = data[OBJECTS_KEY];

    // We can't update the state of the objects right away because
    // Unity will not complete loading the scene until sometime in
    // the future. Changes we made to the objects would revert
    // to how they're defined in the original scene. As a result, we
    // need to run the code after the scene manager reports that a
    // scene has finished loading.

    // To do this, we create a new delegate that contains our object-
    // loading code, and store that in LoadObjectsAfterSceneLoad.
    // This delegate is added to the SceneManager's sceneLoaded
    // event, which makes it run after the scene has finished loading.

    LoadObjectsAfterSceneLoad = (scene, loadSceneMode) => {

        // Find all ISaveable objects, and build a dictionary that maps
        // their Save IDs to the object (so that we can quickly look
        // them up)
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
```

```
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

// Tidy up after ourselves; remove this delegate from the
// sceneLoaded event so that it isn't called next time
SceneManager.sceneLoaded -= LoadObjectsAfterSceneLoad;

// Release the reference to the delegate
LoadObjectsAfterSceneLoad = null;

// And ask the garbage collector to tidy up (again, this will
// cause a performance hitch, but users are fine with this as
// they're already waiting for the scene to finish loading)
System.GC.Collect();
};

// Register the object-loading code to run after the scene loads.
SceneManager.sceneLoaded += LoadObjectsAfterSceneLoad;

return true;
}
```

LoadGame 方法读取文件并首先加载已保存游戏中指定的场景。之后，它创建并保存用于恢复场景中对象状态的代码，并将其添加到 SceneManager 的 sceneLoaded 事件中。该代码本身会找到所有 ISaveable MonoBehaviours，创建一个将它们的 Save ID 映射到对象本身的字典，然后使用加载的对象数据来查找数据的适当对象并将数据传递给该对象。

要保存游戏，你可以在 SavingService 类上调用 SaveGame 方法：

```
// Save the game to a file called "SaveGame.json"
SavingService.SaveGame("SaveGame.json");
```

要加载游戏，你可以调用 LoadGame 方法：

```
// Try to load the game from a file called "SaveGame.json"
SavingService.LoadGame("SaveGame.json");
```





这些方法可以从任何地方调用，但请注意，保存游戏并不是轻量级操作。不要每帧都保存；最多每隔几分钟保存一次。

#### 创建一个可保存的行为

保存系统依赖于对象具有唯一的保存 ID。此 ID 可以手动指定，但没有必要这样做——它永远不会暴露给用户，并且每个 ID 必须唯一。此外，ID 在保存游戏和再次加载游戏之间必须保持不变，这意味着它需要存储在对象本身内部。

因此，最好有一个系统来为你处理此事，所以我们将自己创建一个。我们将对 MonoBehaviour 类进行子类化，并添加代码以在必要时自动生成并公开保存 ID。我们将利用 Unity 允许你在 Unity 将对象序列化到场景中时运行代码的事实，这在你在编辑器中操作对象时经常发生。

此外，我们将使这个子类成为抽象类，这样我们只实现 SaveID 属性，而将 SaveData 和 LoadFromData 方法留给子类实现。

创建一个名为 SaveableBehaviour.cs 的新 C# 脚本，并添加以下代码到其中：

```
public abstract class SaveableBehaviour : MonoBehaviour,
    ISaveable,
    ISerializationCallbackReceiver // Asks Unity to run code when the
    // scene file is saved in the editor
{
    // This class doesn’t implement SavedData or LoadFromData; that’s the
    // job of our subclasses.
    public abstract JsonData SavedData { get; }
    public abstract void LoadFromData(JsonData data);

    // This class does implement the SaveID property; it wraps the _saveID
    // field. (We need to do this manually, rather than using automatic
    // property generation (i.e., "public string SaveID {get;set;}"),
    // because Unity won’t store automatic properties when saving the
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
}
```

```
// The _saveID field stores the actual data that SaveID uses. We mark
// it as serialized so that the Unity editor saves it with the rest of the
// scene, and ask HideInInspector to make it not appear in the Inspector
// (there's no reason for it to be edited).
[HideInInspector]
[SerializeField]
private string _saveID;

// OnBeforeSerialize is called when Unity is about to save this object
// as part of a scene file.
public void OnBeforeSerialize()
{
    // Do we not currently have a Save ID?
    if (_saveID == null)
    {
        // Generate a new unique one, by creating a GUID and getting
        // its string value.
        _saveID = System.Guid.NewGuid().ToString();
    }
}

// OnAfterDeserialize is called when Unity has loaded this object as
// part of a scene file.
public void OnAfterDeserialize()
{
    // Nothing special to do here, but the method must exist to
    // implement ISerializationCallbackReceiver
}
}
```

SaveableBehaviour 类的 OnBeforeSerialize 方法在编辑器中将该类保存到场景时被调用。如果 SaveableBehaviour 在此时没有保存 ID，它会生成一个并将其存储在一个私有变量中（以便其他类无法访问和修改保存 ID；它们不需要知道它），标记为 SerializeField（以便 Unity 将其存储在场景文件中），并设置为 HideInInspector（以便它不会出现在检查器中，因为没有理由编辑它）。

![](img/e54c00769db669e3746fa1e50090241f_404_0.png)

该类是抽象的，这意味着你不能直接创建 SaveableBehaviours 的实例。相反，你对 Saveable Behaviour 进行子类化并实现 SaveData 和 LoadFromData 方法。SaveID 属性会为你处理好。

#### 创建一个可保存的 MonoBehaviour

为了演示如何构建可以保存其数据的脚本，我们将构建一个示例 TransformSaver.cs，它存储其所附加的游戏对象的位移、旋转和缩放信息：

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
```

```
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
}
```

- 2. 要使用此脚本，请向你的游戏对象添加一个 TransformSaver 组件。它将自动包含在保存的游戏中，并在游戏加载时恢复其状态。

#### 讨论

此系统不会自动恢复在场景加载后创建的任何对象。如果要支持此功能，你需要在场景中有一个对象来保存对它创建的对象的引用，并在被要求提供其保存数据时存储有关这些对象的附加信息。当对象被要求加载时，它会重新创建这些对象。

### 13.5 从网络下载和解析 JSON

#### 问题

你希望能够从 Web 服务器下载 JSON 格式的数据，例如配置数据。

#### 解决方案

解决此类问题的步骤有两步：首先，你需要下载数据，其次，你需要解析数据。

![](img/e54c00769db669e3746fa1e50090241f_407_0.png)

此示例使用了 LitJSON，这是配方 13.4 中使用的相同 JSON 处理库。还有许多其他替代方案，如 Json.NET，或对于简单情况，Unity 内置的 JSON 工具。

要从网络下载数据，请使用 UnityWebRequest 类创建请求，然后调用其 SendWebRequest 方法来启动它：

```
// Create the request
UnityWebRequest request = UnityWebRequest.Get("https://httpbin.org/get");

// Send the request; we'll get back an operation that represents its
// work-in-progress state
UnityWebRequestAsyncOperation operation = request.SendWebRequest();
```

当你调用 SendWebRequest 时，它会立即开始与网络通信并返回一个 UnityWebRequestAsyncOperation。这是一个对象，你可以用它来获取有关请求进度的信息。

在大多数情况下，你希望等待操作完成，然后根据其是否成功采取某种行动。

你可以使用操作的 completed 事件来做到这一点：

```
// Tell the operation that we want it to run some code when it's
// finished:
operation.completed += (op) =>
{
// The operation has finished. Now we'll figure out if it succeeded
// or not.
switch (request.result)
{




```case UnityWebRequest.Result.Success:
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
// We connected, but the server didn’t give us data. There
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

下载数据后，你可以使用 LitJSON 来解析 JSON 并提取其中的数据。

在本示例中，我们从 URL https://httpbin.org/get 获取数据。

![](img/e54c00769db669e3746fa1e50090241f_408_0.png)

httpbin 是一个用于测试 HTTP 请求的有用网站，会对多个不同的 URL 返回有用的信息。你可以在 https://httpbin.org 了解更多关于它的信息。

该 URL 返回的数据看起来像这样：

```
{
...
"headers": {
...
"host": "httpbin.org"
}
}
```

在本示例中，我们将获取 "headers" 值内部存储的 "host" 值：

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

#### Discussion

通过在 UnityWebRequest 上调用 SendWeb Request 获得的 UnityWebRequestAsyncOperation 对象也可以在协程中使用：

```
UnityWebRequestAsyncOperation operation = request.SendWebRequest();
StartCoroutine(WaitForDownload(operation));

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

LitJSON 的 JsonMapper.ToObject 方法将 JSON 文本解析为 JsonData 对象，你可以像字典或数组一样访问它（取决于解析的 JSON）。你可以在其官方网站找到 LitJSON 的文档和 API。

### 13.6 Scripted Importers

#### Problem

你想要编写代码，让 Unity 导入一种新型文件。

#### Solution

游戏开发中并非所有内容都围绕相同的[content]

```case UnityWebRequest.Result.Success:
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
// We connected, but the server didn’t give us data. There
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

下载数据后，你可以使用 LitJSON 来解析 JSON 并提取其中的数据。

在本示例中，我们从 URL https://httpbin.org/get 获取数据。

![](img/e54c00769db669e3746fa1e50090241f_408_0.png)

httpbin 是一个用于测试 HTTP 请求的有用网站，会对多个不同的 URL 返回有用的信息。你可以在 https://httpbin.org 了解更多关于它的信息。

该 URL 返回的数据看起来像这样：

```
{
...
"headers": {
...
"host": "httpbin.org"
}
}
```

在本示例中，我们将获取 "headers" 值内部存储的 "host" 值：

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

#### Discussion

通过在 UnityWebRequest 上调用 SendWeb Request 获得的 UnityWebRequestAsyncOperation 对象也可以在协程中使用：

```
UnityWebRequestAsyncOperation operation = request.SendWebRequest();
StartCoroutine(WaitForDownload(operation));

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

LitJSON 的 JsonMapper.ToObject 方法将 JSON 文本解析为 JsonData 对象，你可以像字典或数组一样访问它（取决于解析的 JSON）。你可以在其官方网站找到 LitJSON 的文档和 API。

### 13.6 Scripted Importers

#### Problem

你想要编写代码，让 Unity 导入一种新型文件。

#### Solution

游戏开发中并非所有内容都围绕相同的

// 告诉立方体的 MeshRenderer 使用此材质。
cubeObject.GetComponent<MeshRenderer>().material = cubeMaterial;

// 现在我们将刚刚创建的对象存储为资源。

// 首先，存储 GameObject（使用并渲染网格和材质的组件集合），并将其标记为“主”对象。
ctx.AddObjectToAsset(name, cubeObject);
ctx.SetMainObject(cubeObject);

// 我们还需要存储网格和材质。
ctx.AddObjectToAsset(cubeMaterial.name, cubeMaterial);
ctx.AddObjectToAsset(cubeMesh.name, cubeMesh);

}
}
#endif

这看起来很长很吓人，但它只是一系列小的步骤。它定义了一个结构体 CubeDescription，用来描述立方体是什么：一个 Vector3 和一个 Color。然后它创建了一个 ScriptedImporter，名为 CubeImporter，读取相关文件的 JSON，并使用文件中的立方体信息创建一个新的网格。魔法！

![](img/e54c00769db669e3746fa1e50090241f_413_0.png)

要了解有关 Vector3 和 Unity 中其他数学类型的更多信息，请参阅第 4 章。

+   6. 返回 Unity。你的 Test.cube 文件现在将被导入。它现在包含三个可供 Unity 使用的资源：一个网格、一个材质，以及一个使用网格和材质的游戏对象。你可以像其他模型一样将文件拖入场景。

#### Discussion

启用 Unity 理解你的自定义文件类型打开了一个充满可能性的世界。如果你例如构建了一个导出关卡文件的外部关卡编辑器，该系统将允许你为 Unity 编辑器配备读取你文件的能力。

![](img/e54c00769db669e3746fa1e50090241f_413_1.png)

Unity 社区为 Unity 不一定开箱即支持的许多常见文件格式构建了导入器。如果你缺少文件格式功能，在尝试自己实现之前先在互联网上搜索！
