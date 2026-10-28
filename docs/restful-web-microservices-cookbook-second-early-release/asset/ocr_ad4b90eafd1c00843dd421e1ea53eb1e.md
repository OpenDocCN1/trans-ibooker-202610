Below is an example of a ToDo domain application. This client is implemented by binding to all the actions described in the API documentation.

```
/* to-do-functions.js */
var thisPage = function() {
    function init() {}
    function makeRequest(href, context, body) {}
    function processResponse(ajax, context) {}
    function refreshList() {}
    function searchList() {}
    function addToList() {}
    function completeItem() {}
    function showList() {}
    function attachEvents() {}
};
```

The details of what goes on within each function are not important here. However, it should be noted that when the service adds any new features (e.g. the ability to set a dueDate for a task) this client application will be out-of-date and will not be able to adapt to the new feature. And it would be more challenging if the arguments or other action metadata were changed by the service (e.g. change POST to PUT, modify the URLs, etc.).

Now, look at the following client that is focused on a message-centric coding model. In this case, the control loop is not focused on the “ToDo” domain actions but on the messages the client sends and receives. In fact, this message-centric approach would be the same no matter the service domain or topic.

```
/* to-do-messages.js */
var thisPage = function() {
    function init() {}
    function makeRequest(href, context, body) {}
    function processResponse(ajax, context) {}
    function displayResponse() {}
    function renderControls() {}
    function handleClicks() {}
};
```