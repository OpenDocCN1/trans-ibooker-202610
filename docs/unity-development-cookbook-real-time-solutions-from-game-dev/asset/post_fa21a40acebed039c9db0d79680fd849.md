
Wizards, a long-standing user interface paradigm dating all the way back to the early 1990s, guides you through a sequence of steps. Wizards can be useful when you have a repetitive process you need to frequently perform while building your game.

Let's create a wizard that creates a cube, and also creates a new material that uses a specified color, all with a single click of a button:

- 1. Create a new C# script called CreateCubeWizard.cs, and add the following code to it:

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

We’re defining a ScriptableWizard called CreateCubeWizard that adds a new menu item called “Cube with Color,” allowing us to trigger the wizard easily, and then creates a new cube, asking the user to supply Vector3 for size and a Color for color.

- 2. Open the GameObject menu and note that you’ll see a new menu entry: Cube with Color. Click it.
- 3. Enter your cube size and color, and click Create.
- 4. A new cube will be added to the scene, a new material will be created with the color, and the cube will be set up to use the new material.

### Discussion

Creating editor tools, such as wizards, is an essential part of a more complex Unity workflow. Wizards are a great way of creating custom UI to make your life as a game developer easier.

The Unity Editor API lets you build lots of useful things to make your common workflows easier. If you notice yourself doing something more than a handful of times, it’s often a great idea to use the Unity Editor API to make it easier on yourself. The automation possibilities are extensive!

> **TIP**
> 
> You can learn more about the Unity Editor API in the Unity documentation. There are a lot of possibilities, and a lot of ways you can automate repeated workflows.

## Chapter 13. Files, Networking, and Screenshots

Rich, complex, visual computer programs, like the sort of thing you might make with Unity, also need to do mundane, traditional computer things, like saving, loading, and parsing files. We've saved the most thrilling chapter for last: here you'll find recipes for saving screenshots, state, and textures, as well as for importing files using a custom pipeline. Utterly thrilling (not really), but utterly essential (really!).

### 13.1 Saving Files

### Problem

You want to know where you can save files that your project generates, like screenshots and saved games.

### Solution

Use the Application.persistentDataPath property to get the location of a folder to which you can save data:

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

### Discussion

The directory provided by persistentDataPath is not guaranteed to be exposed to the user; for example, it won't be exposed on mobile platforms where the user doesn't have direct access to the filesystem.

### 13.2 Saving an Image File of Your Game to Disk

### Problem

You want to capture an image of your game (a screenshot), and save it to disk.

### Solution

You can capture a screenshot by using the ScreenCapture.CaptureScreenshot method:

```
// Capturing a screenshot at current resolution
ScreenCapture.CaptureScreenshot("MyScreenshot.png");
```

When you call CaptureScreenshot, Unity will save the resulting image in the directory indicated by Application.persistentDataPath.

The screenshot will be the same size as the screen or window that the game is rendering to. You can also request that the screenshot be larger by providing a scaling factor as a parameter. For example:

```
// Capturing a screenshot at double resolution (e.g., if the screen // resolution is 1920x1080, the resulting file will be 3840x2160)
ScreenCapture.CaptureScreenshot("MyScreenshotDoubleSized.png", 2);
```

### Discussion

When you call CaptureScreenshot, the capture doesn't happen immediately. Instead, Unity waits for the end of the frame when all rendering has completed; at that point, the screenshot is captured and saved to disk.

> **TIP**
> 
> This means that you can capture only one screenshot per frame. Each call to CaptureScreenshot is effectively a “request” to capture a screenshot later, and only one request will be stored at a time.

In addition to saving the screenshot as a file, you can save it as a texture by using the CaptureScreenshotAsTexture method:

```
// Capturing a screenshot as a texture.
// Note that this captures the screenshot immediately, and it may not
// contain what you expect.
Texture2D capturedTexture =
ScreenCapture.CaptureScreenshotAsTexture();
```

Unlike CaptureScreenshot, CaptureScreenshotAsTexture does capture the screenshot immediately, because it needs to return the captured texture. This means that the texture may not contain the final image that you expect.

To make it capture at the end of the frame, you need a coroutine that waits for the end of the frame before capturing the texture, such as in this example:

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

You can run this coroutine by calling it with StartCoroutine, as follows:

```
// Begin the coroutine
StartCoroutine(CaptureScreenshotAtEndOfFrame());
```

### 13.3 Loading Textures from Disk

### Problem

You have an image file stored on disk, and you want to display it in the game.

### Solution

You can load any texture from a file that's stored in a PNG or JPEG image. In this example, we'll place an image in the project folder (not the Assets folder, the folder above it) called ImageToLoad.png:

- 1. In the script that you want to use to perform the loading, add the following code to the top of the file:

```
using System.IO;
```

The System.IO namespace contains a number of useful classes for working with files on the disk.

- 2. Add the following method to the class in which you want to load texture files:

```
// Loads a texture file from disk
public Texture2D LoadTexture(string fileName) {
// Create the path to this file
// Application.dataPath is the path to where the built application
// is; in the editor, it is in the project's root folder (the one
```
