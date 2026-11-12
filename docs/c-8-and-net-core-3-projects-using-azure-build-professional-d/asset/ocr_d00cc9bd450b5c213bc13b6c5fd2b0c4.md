# Define the archiveChat function:

```
function archiveChat () {
    let message = $('#chat')["0"].innerText;
    let archivePath = $('#archivePath').val();
    connection.invoke('ArchiveChat', archivePath, message);
};
```

Like everything else, I've tried to keep this as simple as possible. We take the innerText of our chat panel (div) and the path specified in the archivePath input and pass it through to the server's ArchiveChat method.

Of course, we have a small window for error here: if the user does not type in a valid path for the file to be saved, the code will throw an exception. I'll leave it to your own creativity to sort that one out. I'm just here for the SignalR functionality.

Let's see the outline of the body section:

```
<body>
    <div class="container col-md-10">
        <h1>Welcome to Signal R <label id="user"></label></h1>
    </div>
    <hr />
</body>
</html>
```

The following code will all go sequentially after the horizontal line.

# Naming section

Let's have a look at the naming section first, and then drill into some of the details:

```
<div id="namePanel" class="container">
    <div class="row">
        <div class="col-md-2">
            <label for="username" class="form-label">Username:</label>
        </div>
        <div class="col-md-4">
            <input id="username" type="text" class="form-control" />
        </div>
        <div class="col-md-6">
            <button class="btn btn-default"
                onclick="enterChat()"
            >
                Enter
            </button>
```