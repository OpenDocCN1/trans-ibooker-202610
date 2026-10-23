
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

Once you’ve downloaded the data, you can use LitJSON to parse the JSON and get data out of it.

In this example, we retrieve data from the URL https://httpbin.org/get.

![](img/e54c00769db669e3746fa1e50090241f_408_0.png)

httpbin is a site that’s useful for testing HTTP requests, and responds to a number of different URLs with useful information. You can learn more about it at https://httpbin.org.

The data that this URL will return looks like this:

```
{
...
"headers": {
...
"host": "httpbin.org"
}
}
```

In this example, we’ll fetch the value of the "host" value, which is stored inside the "headers" value:

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

## Discussion

The UnityWebRequestAsyncOperation object that you receive by calling SendWeb Request on a UnityWebRequest can also be used in a coroutine:

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

LitJSON’s JsonMapper.ToObject method parses JSON text into a JsonData object, which you can access like a dictionary or an array (depending on the JSON that you parsed). You can find the documentation and API for LitJSON at its official website.

## 13.6 Scripted Importers

## Problem

You want to write code that lets Unity import a new type of file.

## Solution

Not everything in game development revolves around the same file types. Sometimes you need to make custom file extensions. Unity has you covered!

In this example, we’ll make a custom importer that allows Unity to recognize text files whose filenames end in .cube, which contain JSON data that describes the size and color of a cube.

Let’s begin by creating an asset for which we’ll then write an importer:

- 1. Open your favorite text editor, and create a new, empty file. (If you don’t have a favorite editor, open Visual Studio, and choose New → File. Make a new, empty file.)
- 2. Put the following code in the file:
```
{
  "size": {"x":1, "y":1, "z":2},
  "color": {"r":0.5, "g":1, "b":0.5, "a":1}
}
```
- 3. Save the file as Test.cube in your Unity project’s Assets folder.
- 4. Go to Unity. Note that while your Test.cube file is visible in the Assets folder, it’s not usable in your project, because Unity doesn’t know what to do with .cube files! Let’s fix that.
- 5. Create a new C# script called CubeImporter.cs, and replace the contents with the following code:
```
#if UNITY_EDITOR
using UnityEditor;

// At the time of writing, asset importers are in the Experimental
// module. By the time you’re reading this, the API may have changed, so
// check the documentation.
using UnityEditor.Experimental.AssetImporters;
```

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

```
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

// Create a matrix that scales vertices by the given X, Y, and Z
// amounts.
var scaleMatrix = Matrix4x4.Scale(cubeDescription.size);

// Get a copy of the vertices in the mesh.
var vertices = cubeMesh.vertices;

// For each of these vertices, apply the scale by multiplying
// the matrix against the vertex.
for (int v = 0; v < vertices.Length; v++) {
    vertices[v] = scaleMatrix.MultiplyPoint(vertices[v]);
}

// Store these scaled vertices in the mesh.
cubeMesh.vertices = vertices;

// Tell the cube's MeshFilter to use this new mesh.
cubeObject.GetComponent<MeshFilter>().sharedMesh = cubeMesh;

// Give the mesh a name.
cubeMesh.name = name + " Mesh";

// Create a new material, using the Standard shader (which is
// the default)
var cubeMaterial = new Material(Shader.Find("Standard"));

// Apply the color that we loaded.
cubeMaterial.color = cubeDescription.color;

// Give it a name, too.
cubeMaterial.name = name + " Material";
```
