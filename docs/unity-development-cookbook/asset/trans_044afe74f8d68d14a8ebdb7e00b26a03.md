

```csharp
propertyPosition, property, includeChildren: true);
}

public override float GetPropertyHeight(
    SerializedProperty property, GUIContent label)
{
    // Calculate the height of the help box, given the editor width
    // (the text might wrap over multiple lines)

    float lineSpacing = EditorGUIUtility.standardVerticalSpacing;
    float propertyHeight =
        EditorGUI.GetPropertyHeight(property, includeChildren: true);

    return HelpBoxHeight + lineSpacing + propertyHeight;
}

// Calculates the height of the help box.
private float HelpBoxHeight
{
    get
    {
        var width = EditorGUIUtility.currentViewWidth;
        var helpBoxAttribute = attribute as HelpBoxAttribute;
        var content = new GUIContent(helpBoxAttribute.text);
        float helpBoxHeight =
            EditorStyles.helpBox.CalcHeight(content, width);

        // Add a single line's height to ensure that text doesn't get
        // clipped
        return helpBoxHeight + EditorGUIUtility.singleLineHeight;
    }
}
#endif
```

## Discussion

你也可以在代码中仅将 HelpBoxAttribute 称为 HelpBox，它仍然可以正常工作！

在任何可能被除你之外的人使用的编辑器代码中，使用帮助框都是有礼貌的做法。

![](img/e54c00769db669e3746fa1e50090241f_381_0.png)

你应该假设除了你之外的人会使用你所有的代码，即使你是一个人团队。

## 12.10 资产处理

### Problem

你想要自定义 Unity 导入某些文件的方式。

### Solution

让我们设置一个资源后处理器，自动将文件名以 _n、_nrm 或 _normal 结尾的任何纹理设置为法线贴图：

- 1. 创建一个新的 C# 脚本，命名为 NormalMapTextureImporter.cs，并添加以下代码：

```csharp
#if UNITY_EDITOR
using UnityEditor;

public class NormalMapTextureImporter : AssetPostprocessor
{
    // Called before the texture is imported.
    void OnPreprocessTexture() {
        // Get the name of the file.
        var filename = System.IO.Path.GetFileNameWithoutExtension(assetPath);
        // We're looking for texture files that end in any of these
        // suffixes.
        var normalMapSuffixes = new[] { "_n", "_normal", "_nrm" };
        // Check each one
        foreach (var suffix in normalMapSuffixes) {
            if (filename.EndsWith(suffix)) {
                // Get the texture importer that's currently importing
                // this texture
                TextureImporter textureImporter =
                    assetImporter as TextureImporter;
                // Update its type so that Unity is aware that it's a
                // normal map
                textureImporter.textureType =
                    TextureImporterType.NormalMap;
                // Exit here, since we know we don't need to do any more
                // work
                return;
            }
        }
    }
}
```

```
}
}
}
#endif
```

这段代码创建了一个名为 NormalMap TextureImporter 的 AssetPostprocessor 子类，查找我们想要的后缀，检查每个正在导入的文件是否具有该后缀，并将找到的每个文件的纹理类型更改为法线贴图。

- 2. 为了测试它，取一张包含法线贴图的纹理并重命名，使其名称以 _n 结尾。

![](img/e54c00769db669e3746fa1e50090241f_383_0.png)

如果你手头没有法线贴图，你可以在 NormalMap-Online 找到一个有用的在线法线贴图纹理生成器。

- 3. 将此纹理导入你的项目。文件将被导入为法线贴图！

### Discussion

在游戏开发领域，纹理文件名中有一些常见的事实标准：_d 表示漫反射，_e 表示自发光，_n 表示法线，等等。资源后处理系统允许你基于文件名以及其他参数来处理导入。

你可以使用 OnPreprocessTexture 函数在纹理导入前执行操作，就像我们在这里做的那样。你可能想要更改纹理的某些内容（例如修改其 alpha 值）或更改纹理的压缩格式。

![](img/e54c00769db669e3746fa1e50090241f_383_1.png)

你可以在 Unity 文档中了解更多关于 Unity 的 AssetPostprocessor 类的信息。

还有一个等效的 OnPostProcessTexture 函数，它在资源完成导入时被调用。

![](img/e54c00769db669e3746fa1e50090241f_384_0.png)

处理器不仅适用于纹理，还适用于 Unity 可能导入的任何文件。例如，你可以使用它来重新压缩或重新编码声音文件。

## 12.11 助手向导

### Problem

你想要在 Unity 编辑器中创建并显示一个窗口，让用户提供一些值，并在用户点击按钮时运行代码。

### Solution

助手向导，是一种自 1990 年代初就存在的长期用户界面范式，会引导你完成一系列步骤。当你在构建游戏时需要频繁执行重复过程时，向导会很有用。

让我们创建一个向导，它可以创建一个立方体，并同时创建一个使用指定颜色的新材质，只需单击一个按钮即可：

- 1. 创建一个新的 C# 脚本，命名为 CreateCubeWizard.cs，并向其中添加以下代码：

```csharp
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
```

```
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

    // Finally, make the new cube use this new material.
    newCube.GetComponent<MeshRenderer>().material = tintedMaterial;
}
#endif
```

我们定义了一个名为 CreateCubeWizard 的 ScriptableWizard，它添加了一个名为“Cube with Color”的新菜单项，让我们能够轻松触发向导，然后创建一个新的立方体，要求用户提供 Vector3 的 size 和 Color 的 color。

- 2. 打开 GameObject 菜单，注意你会看到一个新菜单项：Cube with Color。点击它。

- 3. 输入你的立方体大小和颜色，然后点击 Create。

- 4. 一个新的立方体将被添加到场景中，一个新的材质将使用该颜色创建，并且立方体将被设置为使用新的材质。

### Discussion

创建编辑器工具，例如向导，是更复杂的 Unity 工作流程的重要组成部分。向导是创建自定义 UI 以使你的游戏开发生活更轻松的好方法。

Unity Editor API 让你构建大量有用的东西来使你的常见工作流程更轻松。如果你发现自己做某事超过一小把次数，使用 Unity Editor API 使其更轻松通常是一个好主意。自动化可能性非常广泛！

![](img/e54c00769db669e3746fa1e50090241f_386_0.png)

你可以在 Unity 文档中了解更多关于 Unity Editor API 的信息。有很多可能性，有很多方法可以自动化重复的工作流程。

