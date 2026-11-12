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
private IEnumerator WaitForDownload(UnityWebRequestAsyncOperation
webOperation)
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

# 13.6 Scripted Importers

# Problem

You want to write code that lets Unity import a new type of file.