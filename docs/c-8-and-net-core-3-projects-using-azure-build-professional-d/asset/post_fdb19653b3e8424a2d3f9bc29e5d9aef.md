
4. Remember the connection string that we copied earlier; it needs to go into this file:

```json
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

## Creating a client

As we mentioned earlier, the client will consist of JavaScript functions for sending messages to and receiving messages from the server, as well as HTML elements for display.

Add an HTML page to the wwwroot folder and call it index.html:

![](img/762492a8287f369cc1ac4621a212f193_150_0.png)

We'll keep the client page really simple. I'm using div tags as panels to show and hide the different sections of the page. I'm also using bootstrap to make it look nice, but you can design it whichever way you like. I'm also not going to bore you with the basics, such as where to specify your page title. We'll stick to the relevant elements.

We'll create a skeleton page and fill in the functions as we go:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Realtime Chat</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <script src="/lib/browser/signalr.min.js"></script>
    <script type="text/javascript">
    </script>
</head>
```

## Included libraries

Before we start to fill out the functions, let's cover what the link and script tags are doing. They bring in the required libraries:

```html
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
<script src="/lib/browser/signalr.min.js"></script>
```

If you do not want to use bootstrap for the look and feel, you don't need the bootstrap JavaScript library or CSS, but note that we will be using jQuery in our scripts, so leave that one in. We're also referencing the SignalR library from the lib/browser directory that we added it to earlier.

## JavaScript functions

The following code will all go inside the <script> tags.

Our client will need some code to send and consume messages to and from the server. I've tried to keep the JavaScript as simple as possible, opting for jQuery code for readability:

- 1. Create a variable (I've named mine connection) for our SignalR Hub Server and call its start function:

```javascript
var connection = new signalR.HubConnectionBuilder()
.withUrl('/chat')
.build();

connection.start();
```

The '/chat' parameter for .withUrl refers to our Chat.cs class, which inherits the Hub interface from SignalR.

- 2. Add the UpdateChat and Archived methods, which will be invoked by the server:

```javascript
connection.on('UpdateChat', (user, message) => {
	updateChat(user, message);
});
connection.on('Archived', (message) => {
	updateChat('system', message);
});
```

We simply pass the parameters we get from the server onto our updateChat method. We'll define that method further down.

- 3. Define an enterChat function:

```javascript
function enterChat() {
	$('#user').text($('#username').val());
	sendWelcomeMessage($('#username').val());
	$('#namePanel').hide();
	$('#chatPanel').show();
};
```

We set the text of our user label from the value of the username input element, pass it through to our sendWelcomeMessage method (which we'll define in a bit), and toggle the display of the relevant panels.

- 4. Define a sendMessage function:

```javascript
function sendMessage() {
	let message = $('#message').val();
	$('#message').val('');
	let user = $('#user').text();
	connection.invoke('Send', user, message);
};
```

We set the message variable from the message input element, before clearing it for the next message, and the user variable from the user label. Then, we call the Send method on our server by using the connection.invoke method and pass through our variables as parameters.

## 5. Define a sendWelcomeMessage function:

```javascript
function sendWelcomeMessage(user) {
    connection.invoke('Send','system',user+' joined the chat');
};
```

Just like the sendMessage function we described in the previous step, we will use the connection.invoke function to call the Send method on our server. This time, though, we pass through the 'system' string as the user parameter and a little informational message about the user that just joined.

## 6. Define an updateChatPanel method:

```javascript
function updateChatPanel(user, message) {
    let chat = '<b>' + user + '</b>:' + message + '<br/>'
    $('#chat').append(chat);
    if ($('#chat')[0].innerText.length > 0) {
    $('#history').show();
    $('#archivePanel').show();
    }
};
```

updateChat is just our custom function that's used to update the chat history panel. We could have done this inline in the two connection.on functions, but that means we would have repeated ourselves. As a general rule in any coding, you should try not to repeat code.

In this function, we set the chat variable to however we want each chat history line to look in terms of styling. In this case, we simply style our user (with a colon) bold with the message unstyled afterward and a line break at the end. A few lines of chat will look something like this:

```
John: Hello people
Sarah: Hi John
server: Peter joined the chat
John: Hi Sarah, Hello Peter
Peter: Hello Everyone
```

We also need to check the chat div innerText property to determine whether the chat history and archive panels should be visible.

## Define the archiveChat function:

```javascript
function archiveChat () {
    let message = $('#chat')[0].innerText;
    let archivePath = $('#archivePath').val();
    connection.invoke('ArchiveChat', archivePath, message);
};
```

Like everything else, I've tried to keep this as simple as possible. We take the innerText of our chat panel (div) and the path specified in the archivePath input and pass it through to the server's ArchiveChat method.

Of course, we have a small window for error here: if the user does not type in a valid path for the file to be saved, the code will throw an exception. I'll leave it to your own creativity to sort that one out. I'm just here for the SignalR functionality.

Let's see the outline of the body section:

```html
<body>
    <div class="container col-md-10">
        <h1>Welcome to SignalR <label id="user"></label></h1>
    </div>
    <hr />
</body>
</html>
```

The following code will all go sequentially after the horizontal line.

## Naming section

Let's have a look at the naming section first, and then drill into some of the details:

```html
<div id="namePanel" class="container">
    <div class="row">
        <div class="col-md-2">
            <label for="username" class="form-label">Username:</label>
        </div>
        <div class="col-md-4">
            <input id="username" type="text" class="form-control" />
        </div>
        <div class="col-md-6">
            <button class="btn btn-default" onclick="enterChat()">
                Enter
            </button>
        </div>
    </div>
</div>
```

We'll need to know who our chatroom attendee is. Here, we're adding an input element to capture the username and a button to call the enterChat function:

- <input id="username" type="text" class="form-control" />
- <button class="btn btn-default" onclick="enterChat()">Enter</button>

## Chat input

The chat section allows the user to type a message and see what has been typed:

```html
<div id="chatPanel" class="container" style="display: none">
<div class="row">
<div class="col-md-2">
<label for="message" class="form-label">
Message:
</label>
</div>
<div class="col-md-4">
<input id="message" type="text" class="form-control" />
</div>
<div class="col-md-6">
<button class="btn btn-info" onclick="sendMessage()">
Send
</button>
</div>
</div>
</div>
<div id="historyPanel" style="display:none;">
<h3>Chat History</h3>
<div class="row">
<div class="col-md-12">
<div id="chat" class="well well-lg"></div>
</div>
</div>
</div>
</div>
