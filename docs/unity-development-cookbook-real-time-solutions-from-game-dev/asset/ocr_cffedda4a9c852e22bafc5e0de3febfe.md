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

# Discussion