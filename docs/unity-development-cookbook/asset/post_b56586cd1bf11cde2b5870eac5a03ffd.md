
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

You can also call the HelpBoxAttribute just HelpBox in your code, and it will still work just fine!

It’s polite to use help boxes in any editor code that might be used by someone other than you.

![](img/e54c00769db669e3746fa1e50090241f_381_0.png)

You should assume that someone other than you will work with all your code, even if you’re a one-person team.

## 12.10 Asset Processing

### Problem

You want to customize how Unity imports certain files.

### Solution

Let's set up an asset post-processor that automatically sets up any texture whose filename ends in _n, _nrm, or _normal as a normal map:

- 1. Create a new C# script called NormalMapTextureImporter.cs, and add the following code:

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

This code creates a subclass of AssetPostprocessor that we name NormalMap TextureImporter, looks for the suffixes we want, checks each file that’s being imported for that suffix, and changes the texture type to be a normal map for each one it finds.

- 2. To test it, take a texture that contains a normal map and rename it so that its name ends with _n.

![](img/e54c00769db669e3746fa1e50090241f_383_0.png)

If you don’t have any normal maps on hand, you can find a useful online generator of normal map textures at NormalMap-Online.

- 3. Import this texture into your project. The file will be imported as a normal map!

### Discussion

There are some common, de facto standards in filenames for textures in the game development world: _d for diffuse, _e for emissive, _n for normal, and the like. The asset post-processing system allows you to handle imports based on filename, as well as other parameters.

You can use the OnPreprocessTexture function to perform operations before the texture is imported, as we did here. You might want to change something about the texture (for example, modify its alpha value) or change the compression format of the texture.

![](img/e54c00769db669e3746fa1e50090241f_383_1.png)

You can learn more about Unity’s AssetPostprocessor class in the Unity documentation.

There’s also an equivalent OnPostProcessTexture function, which is called when an asset has finished importing.

![](img/e54c00769db669e3746fa1e50090241f_384_0.png)

The processor not only works with textures, but also with any file Unity might be importing. For example, you could use it to recompress or re-encode sound files.

## 12.11 Wizards

### Problem

You want to create and display a window in the Unity editor that lets the user provide some values, and runs code when the user clicks a button.

### Solution

Wizards, a long-standing user interface paradigm dating all the way back to the early 1990s, guides you through a sequence of steps. Wizards can be useful when you have a repetitive process you need to frequently perform while building your game.

Let's create a wizard that creates a cube, and also creates a new material that uses a specified color, all with a single click of a button:

- 1. Create a new C# script called CreateCubeWizard.cs, and add the following code to it:

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

We’re defining a ScriptableWizard called CreateCubeWizard that adds a new menu item called “Cube with Color,” allowing us to trigger the wizard easily, and then creates a new cube, asking the user to supply Vector3 for size and a Color for color.

- 2. Open the GameObject menu and note that you’ll see a new menu entry: Cube with Color. Click it.

- 3. Enter your cube size and color, and click Create.

- 4. A new cube will be added to the scene, a new material will be created with the color, and the cube will be set up to use the new material.

### Discussion

Creating editor tools, such as wizards, is an essential part of a more complex Unity workflow. Wizards are a great way of creating custom UI to make your life as a game developer easier.

The Unity Editor API lets you build lots of useful things to make your common workflows easier. If you notice yourself doing something more than a handful of times, it's often a great idea to use the Unity Editor API to make it easier on yourself. The automation possibilities are extensive!

![](img/e54c00769db669e3746fa1e50090241f_386_0.png)

You can learn more about the Unity Editor API in the Unity documentation. There are a lot of possibilities, and a lot of ways you can automate repeated workflows.
