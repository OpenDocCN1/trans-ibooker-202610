
```
// initializing named variables for service URLs
var serviceURLs = {};
serviceURLs.home = "http://service.example.org/";
serviceURLs.list = "http://service.example.org/list/";
serviceURLs.filter = "http://service.example.org/filter/";
serviceURLs.read = "http://service.example.org/list/{id}/";
serviceURLs.update = "http://service.example.org/list/{id}/";
serviceURLs.remove = "http://service.example.org/list/{id}/";
serviceURLs.approve = "http://service.example.org/{id}/status";
serviceURLs.close = "http://service.example.org/{id}/status";
serviceURLs.pending =
"http://service.api.example.org/{id}/status";

/* later in your client code... */

// using named URL variables
const http = new XMLHttpRequest();

// Send a request
http.open("GET", serviceURLs.list);
http.send();

// handle responses
http.onload = function() {
  switch (http.responseURL) {
    case serviceURLs.home:
      ...
      break;
    ...
  }
}
```

注意，某些 URL 值会被不同的命名变量共享。当重要信息通过 HTTP 请求体和/或 HTTP 方法（例如 HTTP PUT 与 HTTP DELETE）传递时，这种情况很常见。你还会注意到一些 URL 包含模板化值（例如 api.example.org/list/{id}/）。这意味着客户端和服务器必须共享类似的模板 URL 编码标准。我建议使用符合 IETF RFC6570 ³ 的库。

### 从配置加载命名 URL 变量

```
<html>
    <head>
        <script type="text/javascript"
            src="client.example.org/js/service-urls-config.js">
        </script>
        <script type="text/javascript">
            ...
            // using named URL variables
            const http = new XMLHttpRequest();

            // Send a request
            http.open("GET", serviceURLs.list);
            http.send();

            // handle responses
            http.onload = function() {
                switch (http.responseURL) {
                    case serviceURLs.home:
                        ...
                    break;
                    ...
                }
            }
            ...
        </script>
    </head>
    <body>
        ...
    </body>
</html>
```

在上面的例子中，配置文件（看起来与前一示例中的代码完全相同）与客户端代码存储在相同位置（`http://client.example.org/js/`）。然而，它可以存储在任何客户端应用程序能够访问的地方，包括本地文件系统（如果客户端平台支持的话）。

### 通过超媒体将客户端需记忆的 URL 限制为一个

如果你的服务使用能在运行时自动提供 URL 的超媒体格式，你可以将客户端应用需要预先了解的 URL 数量限制为可能的一个 URL：即起始 URL 或“主页” URL。然后你的客户端可以根据在 id、name、rel 和/或 tags 中找到的内联信息（参见 TK）“找到”任何其他 URL。

```
/* find-rel.js */
var startingURL = "http://service.example.org/";
var thisURL = "";
var link = {};

// using named URL variables
const http = new XMLHttpRequest();

// Send a request
http.open("GET", serviceURLs.list);
http.send();

// handle responses
http.onload = function() {
  switch (http.responseURL) {
    case startingURL:
      link = findREL("list");
      if(link.href) {
        thisURL = link.href;
        ...
      }
    ...
    break;
    ...
  }
}
```

最后，如果你能影响服务团队，请尝试说服他们要么 a) 使用超媒体类型，使客户端应用只需记住起始 URL；和/或 b) 让服务器团队提供一个远程配置文件，其中包含定义客户端所需静态 URL 的所有命名变量。

### 讨论

限制服务 URL 变更对客户端影响的最佳方式是将 URL 排除在客户端应用程序代码之外。最有效的做法是让服务使用在运行时提供 URL 的超媒体格式，或者让服务提供包含命名 URL 变量列表的配置信息。如果你无法从服务那里获得这些，那么客户端应用程序程序员就必须尽可能接近这一目标。这意味着使用客户端配置文件，或者如果加载配置数据不可行（例如出于安全原因），则在代码中创建自己的客户端命名变量集合。

> **警告**
>
> 必须认识到，依赖客户端配置文件并不是一种理想的解决方案。服务 URL 的变更需要传达给客户端开发者，以便客户端侧的信息（配置或代码）能够更新。这会造成服务变更与客户端变更之间可能存在延迟。你可以设置客户端对变更进行监控，但这只是增加了更多工作，而且只能缩短客户端应用对无预告变更的“响应时间”。

前面提到，一些 URL 变量可能共享相同的 URL 数据，但用途却截然不同。例如，用于读取、更新和删除的服务 URL 可能是相同的（*service.example.org/{id}*）。这促使客户端应用更进一步，也在代码中保存完整的请求元数据集合（URL、方法、标头、查询字符串和/或请求体信息）。有关此选项及其使用时机，请参阅[配方 3.10]。

### 相关配方

- TK（超媒体）
- 配方 3.10
- 参见 *第4章* 中的 XXX

## 3.2 使客户端代码具备 HTTP 感知能力

有时候，辅助库、包装器和/或 API 软件开发工具包（SDK）非但不能让 API 客户端应用更轻松地解决问题，反而可能让其*更困难*。因此，确保你的 API 客户端应用能够使用 HTTP（或任何目标协议）与服务“对话”是很重要的。

### 问题

```javascript
/**************************************************************
 *** PUBLIC METHODS ***
     args = {url:string, headers:{}, queryString:{}, body:{},
             callback:function, context:string}
 **************************************************************/
httpGet: function(args) {...},
httpGetXML: function(args) {...},
httpGetJSON: function(args) {...},
httpGetPlainText: function(args) {...},
httpPost: function(args) {...},
httpPostVars: function(args) {...},
httpPostFormXML: function(args) {...},
httpPostForJSON: function(args) {...},
httpPostForPlainText: function(args) {...},
httpPut: function(args) {...},
httpHead: function(args) {...},
httpDelete: function(args) {...},
httpOptions: function(args) {...},
httpTrace: function(args) {...},

// implementation details below ...
}
```

然后可以使用以下代码调用该库：

```javascript
function getDocument(url) {
    var args = {};
    args.url = url;
    args.callbackFunction = ajaxComplete;
    args.context = "processLinks";
    args.headers = {'accept':'application/vnd.collection+json'}

    ajax.httpGet(args)

    // later ...

function ajaxComplete(response,headers,context,status,msg)
{
    switch(status) {...} // handle status
    switch(context) {...} // dispatch to context
}
```

上述示例是来自面向基于浏览器应用的客户端 JavaScript 实现的伪代码。你的库的具体细节并不重要。重要的是你的客户端应用能够快速、安全地构造一个 HTTP 请求并处理

下面是一个 ToDo 领域应用程序的示例。该客户端通过绑定 API 文档中描述的所有操作来实现。

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

此处每个函数内部的具体细节并不重要。但应当注意，当服务添加任何新功能（例如为任务设置 dueDate 的能力）时，该客户端应用将过时，无法适应新功能。而且，如果服务更改了参数或其他操作元数据（例如将 POST 改为 PUT、修改 URL 等），挑战会更大。

现在，看看下面这个以消息为中心的编码模型的客户端。在这种情况下，控制循环关注的不是“ToDo”领域操作，而是客户端发送和接收的消息。事实上，无论服务领域或主题是什么，这种以消息为中心的方法都是相同的。

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

