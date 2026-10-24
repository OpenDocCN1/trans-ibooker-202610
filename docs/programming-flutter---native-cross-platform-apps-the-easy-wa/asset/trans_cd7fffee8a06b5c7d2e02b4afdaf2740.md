

现在我们需要检查文件是否存在且不为空，并返回其解码后的内容：

```dart
if(await comicFile.exists() && comicFile.readAsStringSync() != "")
  return json.decode(comicFile.readAsStringSync());
```

如果不存在，我们需要创建文件并将 JSON 字符串写入其中：

```dart
else {
  final comic =
    await http.read('https://xkcd.com/${latestComic - n}/info.0.json');
```

将整个函数放在一起后，最终结果为：

```dart
Future<Map<String, dynamic>> _fetchComic(int n) async {
  final dir = await getTemporaryDirectory();
  int comicNumber = latestComic-n;
  var comicFile = File("${dir.path}/$comicNumber.json");
  if(await comicFile.exists() && comicFile.readAsStringSync() != "")
    return json.decode(comicFile.readAsStringSync());
  else {
    final comic =
      await http.read('https://xkcd.com/${latestComic - n}/info.0.json');
    /* no need to use sync methods as we
       don’t have to wait for it to finish caching */
    comicFile.writeAsString(comic);
    return json.decode(comic);
  }
}
```

由于 SelectionPage 也应该利用缓存的漫画，我们只需稍微修改此函数，使其将漫画编号作为字符串参数接收：

```dart
Future<Map<String, dynamic>> _fetchComic(int n) async {
  final dir = await getTemporaryDirectory();
  var comicFile = File("${dir.path}/$n.json");

  if(await comicFile.exists() && comicFile.readAsStringSync() != "")
    return json.decode(comicFile.readAsStringSync());
  else {
    final comic =
      await http.read('https://xkcd.com/$n/info.0.json');
    comicFile.writeAsString(comic);
    return json.decode(comic);
  }
}
```

## 保存最新的漫画编号

在当前状态下，我们的应用程序仍然依赖于启动时获取最新的漫画编号才能工作，但我们可以将该编号在获取时保存到文件中，如果在某些情况下启动时无法获取，我们将尝试从文件中获取。

我们显然希望在可能的情况下始终获取远程编号，因为我们希望该编号在出现新漫画时发生变化，这与我们在 `fetchComic` 中所做的不同，在那里我们尽可能从文件中获取，仅在不可能时才求助于从互联网获取。

要在我们的应用程序中实现这一点，我们需要更改 `getLatestComicNumber` 顶层函数，并且为实现我们想要的功能，有一个大多数程序员熟悉的功能：使用 try 和 catch 捕获异常。

如果应用程序尝试网络连接并失败（当设备因任何原因无法访问站点时就会发生这种情况），它会抛出异常。

我们可以捕获该异常并从本地存储中获取漫画编号。

我们将从声明目录和文件变量开始，同时声明一个将返回的最新漫画编号的变量：

```dart
final dir = await getTemporaryDirectory();
var file = File('${dir.path}/latestComicNumber.txt');
int n = 1;
```

## 捕获异常

本节是对 *抛出和捕获异常* 的预演，因为我们将处理 `try-catch` 块来捕获异常。

无论在任何编程语言中，只要某段代码未能执行所要求的操作，编程语言就会允许它 *抛出* 一个 *异常*。这会导致相关软件崩溃，但任何异常都可以通过 *捕获* 它并对异常做出响应来处理。

例如，如果我们尝试获取漫画编号并抛出异常（意味着最新漫画编号无法获取，很可能是因为没有网络连接），我们可以捕获该异常并检查我们是否在之前的某个时间获取过它，以便从那里检索它。

为此，我们将把可能失败的内容包装在 try 块中，如下所示：

```dart
try {
  // CODE THAT COULD FAIL
} catch(exceptionDetails) {
  print("Something failed");
  print(exceptionDetails);
}
```

那么让我们在 try 块中获取最新的漫画编号：

networking/xkcd_app/lib/main.dart

```dart
try {
  n = json.decode(
    await http.read('https://xkcd.com/info.0.json')
  )["num"];
}
```

由于将编号保存到文件不会影响函数其余部分的执行，我们可以检查文件是否存在，如果不存在则创建它并将编号写入文件，而无需让函数等待这些操作完成（所有这些都将在 try 块内，并且只有在前一条指令未失败时才会执行）：

networking/xkcd_app/lib/main.dart

```dart
file.exists().then(
  (exists) {
    if(!exists) file.createSync();
    file.writeAsString('$n');
  }
);
```

.then 方法创建一个新线程，等待函数返回一个值。

当 file.exists 返回一个值时，它会传递给一个匿名函数，该函数执行我们需要完成的所有操作，而不会影响函数的其余部分。

如果这失败并且存在可以获取编号的文件，我们将使用 catch 关键字从该文件获取编号：

networking/xkcd_app/lib/main.dart

```dart
catch(e) {
  if(
    file.existsSync() &&
    file.readAsStringSync() != ""
  )
    n = int.parse(file.readAsStringSync());
}
```

并且，无论我们如何到达这里，我们都返回编号：

networking/xkcd_app/lib/main.dart

```dart
return n;
```

## 保存图片

现在，如果你尝试在没有网络连接的情况下打开应用程序，你会注意到一切都正常工作，但没有图片。

这并不令人惊讶：我们只保存了它们的 URL，而显然在没有网络连接的情况下无法访问图片。

这意味着我们需要下载图片以使我们的应用程序 100% 离线可用，并且为了能够做到这一点，我们将再次不得不更改 fetchComic。

当文件已存在时运行的代码不需要更改：它只是读取文件并保存它，我们只需要确保在运行应用程序之前清除缓存，因为我们将更改漫画 JSON 字符串的保存方式，并且我们将更改使用缓存漫画中值 的函数。

在 HomeScreen 的 fetchComic 方法的 else 语句的花括号中，我们需要创建文件，获取漫画的 JSON 正文，将图片保存到本地存储，并用保存的图片路径替换 img 参数中的 URL。

这些都是本书已经描述过的操作，当放在一起时，产生以下 _fetchComic：

networking/xkcd_app/lib/main.dart

```dart
Future<Map<String, dynamic>> _fetchComic(int n) async {
  final dir = await getTemporaryDirectory();
  int comicNumber = latestComic-n;
  var comicFile = File('${dir.path}/$comicNumber.json');
  if(
    await comicFile.exists() &&
    comicFile.readAsStringSync() != ""
  )
    return json.decode(comicFile.readAsStringSync());
  else {
    comicFile.createSync();
    final comic = json.decode(
      await http.read(
        'https://xkcd.com/$comicNumber/info.0.json'
      )
    );

    File('${dir.path}/$comicNumber.png')
      .writeAsBytesSync(await http.readBytes(comic["img"]));
    comic["img"] = '${dir.path}/$comicNumber.png';
    comicFile.writeAsString(json.encode(comic));

    return comic;
  }
}
```

我们只是使用一个没有 await 表达式的异步函数来写入漫画文件，因为它在应用程序重启之前不会被使用，所以我们不需要像例如对图像文件那样暂停执行来写入它，图像文件将在方法返回后不久被 ComicTile 使用。

SelectionPage 的 _fetchComic 方法也适用完全相同的更改，因为此部分函数在这两者之间没有变化：

networking/xkcd_app/lib/main.dart

```dart
Future<Map<String, dynamic>> fetchComic(String n) async {
final dir = await getTemporaryDirectory();
var comicFile = File("${dir.path}/$n.json");

if(
await comicFile.exists() &&
comicFile.readAsStringSync() != ""
)
return json.decode(comicFile.readAsStringSync());
else {
comicFile.createSync();
final comic = json.decode(
await http.read("https://xkcd.com/$n/info.0.json")
);

File("${dir.path}/$n.png")
.writeAsBytesSync(await http.readBytes(comic["img"]));
comic["img"] = "${dir.path}/$n.png";
comicFile.writeAsString(json.encode(comic));

return comic;
}
}
```

由于 `comic["img"]` 的值现在是包含漫画图片的文件的路径，我们需要在 `ComicPage` 和 `ComicTile` 的 build 方法中将以下行更改为：

```dart
Image.network(comic["img"])
```

改为：

```dart
Image.file(File(comic["img"]))
```

此时，使用活动互联网连接打开应用程序，关闭它然后在没有互联网连接的情况下再次打开它，将显示应用程序现在完全能够浏览已在获取时浏览过的漫画，而无法向 XKCD 网站发出请求。

