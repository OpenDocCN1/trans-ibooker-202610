```
// A CubeDescription contains the variables that define our cubes. We'll
// create them by loading them from text files that contain JSON.
public struct CubeDescription {
    public Vector3 size;

    // storing the r, g, b, a values in this 4-component vector
    public Color color;
}

// Indicate to Unity that this script imports files with the file
// extension ".cube," and that this is version 0 of the importer
// (changing the number makes Unity reimport assets of this type).
[ScriptedImporter(0, "cube")]
public class CubeImporter : ScriptedImporter {

    // Called by Unity to perform an import
    public override void OnImportAsset(AssetImportContext ctx)
    {

        // "ctx" contains information about the import that Unity wants
        // us to do; it contains the path to the file, and we'll put
        // the Unity objects into it when we're done

        // "cube" files will contain JSON that describes the color and
        // size of the cube.

        // Create a variable to load the cube description into
        CubeDescription cubeDescription;

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
        catch (System.Exception e) {
            // We caught some other kind of exception, and can't
            // continue. Rethrow the error.
            throw e;
        }
    }
```