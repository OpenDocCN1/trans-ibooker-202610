

## Part 2
### 使用 Flutter 做更多事情

Flutter 可扩展且灵活。我们已经了解了它的工作原理，现在我们将看看 Flutter 的众多功能如何被用来实际构建有用的应用。

本部分各章中你将学习如何进行 HTTP 请求，如何在 Flutter 应用中进行测试和处理异常，然后如何使用 Firebase 创建一个聊天应用。

附录将介绍 Dart 语法、Cupertino（类似 iOS）组件、pubspec.yaml，以及如何更改应用名称和启动图标。

## Chapter 5
### 网络和存储 I/O 与导航

编写仅限于用户自己手机的应用对于某些应用来说是可以的，但绝大多数应用都需要与 Web API 或后端交互，这就是本章要做的事情。

Flutter 的标准库不包含标准网络组件：即使你实际上可以使用网络来获取图像，但仅使用 Flutter/Dart 标准库就无法进行标准的 HTTP 请求。

这就是我们现在要谈论它的原因：Flutter 在 Pub 上有一个由 Google 开发的包叫做 *http*，它允许我们进行 HTTP 请求并将我们的应用连接到 Web API。

## The JSON Data

如果你从未使用过 JSON，它只是一种用字符串描述任何面向对象编程语言中的对象（或更接近地，C 风格的结构）的方式：它是一个包含多个字段的单一实体，每个字段都描述了该实体不同的方面。

在本例中，它是 XKCD 最新发布的漫画以及关于它的以下信息：

- 发布月份（1 代表一月）。
- 漫画的数字 ID（递增，从 1 开始，即第一部 XKCD 漫画）。
- 点击网站上的漫画图像时指向的链接 [33]（通常为空）。
- 漫画发布的年份。
- 通常为空的 *news* 字段。
- 漫画的纯文本标题（正常的 *title* 可能包含 HTML 标签）。
- 文本形式的转录（描述）漫画图像中发生的事情，自第 1674 号漫画以来就未再添加。
- *alt* 是对笑话的解释或在观看漫画图像后才应查看的附加文本。它已在官方网站上以两种方式包含：桌面版在鼠标悬停在图像上时显示它，移动版在点击图像下方的按钮时显示它。
- 漫画图像本身。
- 可能包含 HTML 标签的漫画标题。
- 漫画发布的月份中的日期。

在 Dart 代码中，我们可以使用 dart:convert 内置 Dart 库解析和解码 JSON 数据并将其存储在 Dart Map 中，该库提供了必要的 json.decode(JSON) 方法将 JSON 字符串转换为 Dart 对象，以及一个 json.decode(var) 方法（我们不会使用），它将兼容的 Dart 对象转换为 JSON 格式的字符串。

### How the Convert Library Works

dart:convert 库提供了 json 常量，[34] 其类型为 JsonCodec，[35] 它有两个方法：json.decode，它接受一个 String 作为参数，可以返回任何 Dart 对象，因为其返回类型为 dynamic，意味着它可以是任何东西。

> 另一个方法是 json.encode，它做完全相反的事情：它接受任何 Dart 对象（同样，它可以是任何东西）并返回一个 String，可以用作——例如，如果我们需要的话，作为 POST 请求的一部分，而我们在本书后面也会这样做。

## Getting Previous Comics

你可以通过向 https://xkcd.com/num/info.0.json 发送 get 请求来获取当前（最新）漫画之前的漫画，其中 num 是按时间顺序的漫画编号（从 1 开始）；例如，你可以通过向 https://xkcd.com/1/info.0.json 发送 GET 请求来获取第一部漫画。

API 没有办法获取漫画列表，但我们可以通过简单地获取最新漫画，然后使用其 ID 号（我们称之为 latestNum）来获取编号从 latestNum-1 到 1 的漫画，从而按逆时间顺序（从最新到最早）制作漫画列表。

## The http Package

要进行 GET 请求，我们需要使用 *http* 包。
你可以在 Dart Pub 上找到 http 包。[^36]

让我们开始通过使用以下命令创建一个新的 Flutter 应用项目：

```
```bash
$ flutter create xkcd_app
```
```

并通过在 `pubspec.yaml` 中添加以下内容来安装 HTTP 包：

```
```yaml
# networking/xkcd_app/pubspec.yaml
dependencies:
  http: "^0.12.0+1"
```
```

如果未使用 IDE 插件（在你对 `pubspec.yaml` 进行更改后它们会自动为你执行此操作），请运行 flutter packages get 以下载并安装该包。

### USING THE HTTP PACKAGE

HTTP 包可用于进行各种 HTTP 请求。[^37]

它们都是异步的，这意味着默认情况下，它们在单独的线程中运行，而不会影响应用其余部分的执行。你可以选择让它们的行为与同步函数完全一样，如果它们的返回值对应用执行的进行至关重要。

#### Asynchronous Code in Dart

在 Dart 中运行异步代码有许多方法，概述在 *Asynchronous Code in Dart: The dart:async Library* 中，但基本概念已在上一章中解释过：调用返回 `Future`（或 `Stream`，本章我们不会使用）的函数并在单独线程中运行的调用称为异步。

我们可以通过使用 await 表达式获取异步函数返回的数据：

```
var returnedData = await asyncFunction();
```

这只能在 async 函数中完成，而 async 函数必须返回 Futures，因此异步性往往会传播到所有调用方法。

## Making GET Requests

HTTP 请求使用 http.get 进行，它是一个 async 方法，返回一个包含在 Future 中的 Response。

我们可以使用 await 解包内部的 Response：

```
var latestComicResponse = await http.get("https://xkcd.com/info.0.json");
```

我们可以通过查询其 body 属性来获取响应的主体作为 String，就像我们在构建 XKCD 应用时需要做的那样：

```
var latestComicString = latestComicResponse.body;
```

我们也可以在一行中完成所有操作，将 await 表达式括在括号中：

```
var latestComicString = (await http.get("https://xkcd.com/info.0.json")).body;
```

HTTP 头可以通过将 headers 命名参数设置为 Map<String, String> 来设置，如以下示例所示：

```
http.get(
  "http://example.com/exampleGet?testAttr=$val1&testAttr2=$val2",
  headers: {
    "Accept": "application/json",
    "Accept-Charset": "utf-8"
  }
)
```

Wikipedia 有一份标准 HTTP 头的全面列表。[38]

`http` 包还提供了 `http.read()` 和 `http.readBytes()`，它们分别就像 `http.get()` 的 `body` 和 `bodyBytes` 属性。

例如，你可以从调用：

```
String res = (await http.get("http://example.com")).body;
```

改为：

```
String res = await http.read("http://example.com");
```

或从：

```
String res = (await http.get("http://example.com")).bodyBytes;
```

改为：

```
String res = await http.readBytes("http://example.com");
```

不过要小心，因为你会丢失 `Response` 对象的其他属性（`http.get` 返回的类型），例如：

- `headers`，当你需要读取响应头时需要它；
- `contentLength`，它是响应的大小（以字节为单位）。

## Making Other Requests

也支持其他种类的 HTTP 请求，它们都支持我们刚才看到的设置头的方式，并且它们都返回 `Future<Response>` 对象：

- POST 请求使用 `http.post` 进行，请求 URL 作为位置参数提供，你可以通过将 `body` 命名参数设置为你想要的任何内容来设置请求体。
- PUT 请求使用 `http.put` 进行，请求 URL 作为位置参数提供，你可以通过将 `body` 命名参数设置为你想要的任何内容来设置请求体。
- DELETE 请求使用 `http.delete` 进行，请求 URL 作为位置参数提供。
- PATCH 请求使用 `http.patch` 进行，请求 URL 作为位置参数提供，你可以通过将 `body` 命名参数设置为你想要的任何内容来设置请求体。
- HEAD 请求使用 `http.head` 进行，请求 URL 作为位置参数提供。

