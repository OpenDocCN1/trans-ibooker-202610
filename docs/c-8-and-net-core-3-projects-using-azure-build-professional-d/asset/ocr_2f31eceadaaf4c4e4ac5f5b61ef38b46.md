4. Remember the connection string that we copied earlier; it needs to go into this file:

```
{
"Azure": {
"SignalR": {
"ConnectionString":
"Endpoint=https://signalr-realtimechat.service.signalr.net;AccessKey=accesskeyhere;Version=1.0;"
}
}
}
```

Make sure that you update the connection string to reflect your connection string.

We can (and will) extend our server's functionality a bit later, but for now, let's head over to our client.

# Creating a client

As we mentioned earlier, the client will consist of JavaScript functions for sending messages to and receiving messages from the server, as well as HTML elements for display.

Add an HTML page to the wwwroot folder and call it index.html: