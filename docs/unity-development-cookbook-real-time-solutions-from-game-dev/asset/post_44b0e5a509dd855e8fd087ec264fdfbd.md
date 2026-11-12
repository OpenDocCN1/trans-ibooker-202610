
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

2. To use this script, add a TransformSaver component to your game object. It will automatically be included in saved games, and its state will be restored when the game is loaded.

## Discussion

This system doesn't automatically restore any objects that were created after the scene loaded. If you want to support this, you'll need to have an object in your scene that keeps references to objects that it creates, and stores the additional information about those objects when it's asked for its save data. When the object is asked to load, it re-creates the objects.

## 13.5 Downloading and Parsing JSON from the Web

### Problem

You want to be able to download data in JSON format from a web server, such as configuration data.

### Solution

There are two steps to solving this kind of problem: first, you need to download the data, and second, you need to parse the data.

> **NOTE**
> This example makes use of LitJSON, which is the same JSON handling library used in Recipe 13.4. There are many other alternatives out there, like Json.NET, or for simple cases, Unity's built-in JSON tools.

To download data from the web, use the UnityWebRequest class to create a request, and then call its SendWebRequest method to start it:

```
// Create the request
UnityWebRequest request = UnityWebRequest.Get("https://httpbin.org/get");

// Send the request; we'll get back an operation that represents its
// work-in-progress state
UnityWebRequestAsyncOperation operation = request.SendWebRequest();
```

When you call SendWebRequest, it starts talking to the network and immediately returns a UnityWebRequestAsyncOperation. This is an object that you can use to get information about the request's progress.

In most cases, you'll want to wait until the operation completes, and then (depending on whether it succeeded or not) take some kind of action.

You can do this using the operation's completed event:

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

Once you've downloaded the data, you can use LitJSON to parse the JSON
and get data out of it.

In this example, we retrieve data from the URL
https://httpbin.org/get.

> **NOTE**
>
> httpbin is a site that's useful for testing HTTP requests, and responds to a number of different URLs with useful information. You can learn more about it at https://httpbin.org.

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

In this example, we'll fetch the value of the "host" value, which is stored inside the "headers" value:

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

The UnityWebRequestAsyncOperation object that you receive by calling SendWebRequest on a UnityWebRequest can also be used in a coroutine:

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

LitJSON's JsonMapper.ToObject method parses JSON text into a JsonData object, which you can access like a dictionary or an array (depending on the JSON that you parsed). You can find the documentation and API for LitJSON at its official website.

## 13.6 Scripted Importers

### Problem

You want to write code that lets Unity import a new type of file.

### Solution

Not everything in game development revolves around the same file types. Sometimes you need to make custom file extensions. Unity has you covered!

In this example, we'll make a custom importer that allows Unity to recognize text files whose filenames end in .cube, which contain JSON data that describes the size and color of a cube.

Let's begin by creating an asset for which we'll then write an importer:

- 1. Open your favorite text editor, and create a new, empty file. (If you don't have a favorite editor, open Visual Studio, and choose New → File. Make a new, empty file.)
- 2. Put the following code in the file:

```
{
    "size": {"x":1, "y":1, "z":2},
    "color": {"r":0.5, "g":1, "b":0.5, "a":1}
}
```

- 3. Save the file as Test.cube in your Unity project's Assets folder.

- 4. Go to Unity. Note that while your Test.cube file is visible in the Assets folder, it's not usable in your project, because Unity doesn't know what to do with .cube files! Let's fix that.

- 5. Create a new C# script called CubeImporter.cs, and replace the contents with the following code:

```
#if UNITY_EDITOR
using UnityEditor;

// At the time of writing, asset importers are in the Experimental
// module. By the time you're reading this, the API may have changed, so
// check the documentation.
```
