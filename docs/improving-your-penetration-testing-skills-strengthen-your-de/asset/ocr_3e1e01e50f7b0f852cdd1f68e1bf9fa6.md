```
{
    if (xmlHttp.readyState==4)
    {
        showContents(xmlhttp.ResponseText);
    }
    //GET method to get the links.txt file
    xmlHttp.open("GET", "links.txt", true);
```

The function loadfile() first instantiates the xmlhttp object. It then uses this object to pull a text file from the server. When the text file is returned by the server, it displays the contents of the file. The file and its contents are loaded without user involvement, as shown in the preceding code snippet.

# HTML5

The fifth version of the HTML specification was first published in October 2014. This new version specifies APIs for media playback, drag and drop, web storage, editable content, geolocation, local SQL databases, cryptography, web sockets, and many others, which may become interesting from the security testing perspective as they open new paths for attacks or attempt to tackle some of the security concerns in previous HTML versions.

# WebSockets

HTTP is a stateless protocol as noted previously. This means that a new connection is established for every request and closed after every response. An HTML5 WebSocket is a communication interface that allows for a permanent bidirectional connection between client and server.

A WebSocket is opened by the client through a GET request such as the following:

```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://example.com
```