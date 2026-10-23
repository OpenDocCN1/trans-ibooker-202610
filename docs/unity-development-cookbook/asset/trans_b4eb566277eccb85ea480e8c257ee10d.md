

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

## Discussion

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

## 13.6 Scripted Importers

## Problem

你想要编写代码，让 Unity 导入一种新型文件。

## Solution

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

## Discussion

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

## 13.6 Scripted Importers

## Problem

你想要编写代码，让 Unity 导入一种新型文件。

## Solution

游戏开发中并非所有内容都围绕相同的
