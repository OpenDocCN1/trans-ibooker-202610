# 12.10 Asset Processing

# Problem

You want to customize how Unity imports certain files.

# Solution

Let's set up an asset post-processor that automatically sets up any texture whose filename ends in _n, _nrm, or _normal as a normal map:

+   1. Create a new C# script called NormalMapTextureImporter.cs, and add the following code:

```
#if UNITY_EDITOR
using UnityEditor;

public class NormalMapTextureImporter : AssetPostprocessor
{

    // Called before the texture is imported.
    void OnPreprocessTexture() {

        // Get the name of the file.
        var filename =
            System.IO.Path.GetFileNameWithoutExtension(assetPath);

        // We're looking for texture files that end in any of these
        // suffixes.
        var normalMapSuffixes = new[] { "_n", "_normal", "_nrm" };

        // Check each one
        foreach (var suffix in normalMapSuffixes) {
            if (filename.EndsWith(suffix)) {

                // Get the texture importer that's currently importing
```