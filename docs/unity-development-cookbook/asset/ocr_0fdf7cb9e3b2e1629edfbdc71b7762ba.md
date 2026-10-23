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

+   2. Open the GameObject menu and note that you’ll see a new menu entry: Cube with Color. Click it.