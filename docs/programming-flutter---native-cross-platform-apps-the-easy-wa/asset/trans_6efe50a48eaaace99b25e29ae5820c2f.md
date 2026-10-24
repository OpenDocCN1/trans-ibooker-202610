

`getLatestComicNumber` 方法是我们之前在《A Function to Get the Latest Comic Number》中创建的，我们需要在文件开头定义它，位于库导入之后并且在任何类定义之外：

```
networking_starting/xkcd_app/lib/main.dart

Future<int> getLatestComicNumber() async =>
  json.decode(
    await http.read('https://xkcd.com/info.0.json')
  )
  ['num'];
```

## DEFINE THE HOME SCREEN

此时我们需要定义 HomeScreen 小部件。这次应用树非常简单，我们不需要图示来了解整体情况：HomeScreen 将是一个普通的 Scaffold，始终带有 AppBar，而其 body 将是一个由 ListTile 组成的 ListView。

由于每个漫画都需要一个 ListTile，我们将使用 ListView.builder 构造函数，如《The ListView》中所述，它使用 itemBuilder 函数来生成每个小部件。在本例中，itemBuilder 将调用一个 FutureBuilder，它是一种根据 Future 的内容构建小部件的 item builder，而这正是我们需要的，因为 http.get 是一个返回 Future 的异步函数。

让我们开始编写类定义的基础部分：它将是一个 StatelessWidget，并且需要两个参数——应在 AppBar 中显示的标题和最新的漫画编号：

```
networking_starting/xkcd_app/lib/main.dart

class HomeScreen extends StatelessWidget {
  HomeScreen({this.title, this.latestComic});
  final int latestComic;
  final String title;
}
```

此时我们将通过定义一个 `_fetchComic` 方法来完成对该类的准备工作，就像我们在《The New fetchComic》中讨论的那样：

```
networking_starting/xkcd_app/lib/main.dart
```

```
Future<Map<String, dynamic>> _fetchComic(int n) async =>
  json.decode(
    await http.read(
      "https://xkcd.com/${latestComic-n}/info.0.json"
    )
  );
```

现在剩下的就是有趣的部分：在 build 方法中定义我们想要的应用 UI。我们将从定义 Scaffold 和 AppBar 开始，就像我们一直做的那样：

```
networking_starting/xkcd_app/lib/main.dart
```

```
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text(title),
    ),
  );
}
```

Scaffold 的 body 将是，如我们所说，一个由 ListView.builder 构建的 ListView，我们需要为它提供两个参数：

- itemCount，即将要构建的最大项目数；在本例中，它将是 latestComic 编号，因为这也是迄今为止发布的漫画数量。
- itemBuilder，即一个函数，它接收 BuildContext 和一个从顶部开始的 int，值为 0（我们称之为 i）。

在本例中，itemBuilder 将是，如我们之前所说，一个 FutureBuilder。

FutureBuilder 本身需要两个参数：future，即将为每个 tile 检索数据的 Future，以及 builder 函数，它根据 BuildContext 和 Future 的结果构建 tile。

future 将是对 fetchComic(i) 的调用，其中 i 是上述递增的整数变量，而 itemBuilder 将返回一个 ComicTile，它将漫画数据作为参数。我们稍后再担心 ComicTile 的实现。

## On the Correct Use of the FutureBuilder

FutureBuilder 的 future 不应是函数调用：它应该是之前获取的 Future，因为将其设置为函数调用会导致在父小部件每次重建时 future 都会被调用，但这在我们的案例中不适用：父小部件是一个 StatelessWidget，且上方没有 StatefulWidgets。不过你在构建自己的 Flutter 应用时需要记住这一点：忘记这一点可能会使应用交互变得不必要地缓慢和笨拙。

通常这些 Future 应该在 StatefulWidgets 的 initState 方法中获取。

以下是 HomeScreen 的 Scaffold 的完整 body：

# networking_starting/xkcd_app/lib/main.dart

```
body: ListView.builder(
  itemCount: latestComic,
  itemBuilder:(context, i) => FutureBuilder(
    future: _fetchComic(i),
    builder: (context, comicResult) =>
      comicResult.hasData ?
      ComicTile(comic: comicResult.data) :
      Divider();
  ),
),
```

我们之前看到的代码片段组合起来的完整 HomeScreen 类定义是：

# networking_starting/xkcd_app/lib/main.dart

```
class HomeScreen extends StatelessWidget {
  HomeScreen({this.title, this.latestComic});
  final int latestComic;
  final String title;

  Future<Map<String, dynamic>> _fetchComic(int n) async =>
  json.decode(
    await http.read(
      "https://xkcd.com/$latestComic-n/info.0.json"
    )
  );

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
```

```
),
body: ListView.builder(
  itemCount: latestComic,
  itemBuilder:(context, i) => FutureBuilder(
    future: _fetchComic(i),
    builder: (context, comicResult) =>
      comicResult.hasData ?
      ComicTile(comic: comicResult.data) :
      Divider();
  ),
);
}
}
```

## DEFINE THE COMICTILE

现在我们需要定义 ComicTile 类，它将是另一个 StatelessWidget，并且它将把需要显示的漫画数据作为其唯一参数接收：

networking_starting/xkcd_app/lib/main.dart

```
class ComicTile extends StatelessWidget {
  ComicTile({this.comic});

  final Map<String, dynamic> comic;
```

ComicTile 将是一个 ListTile，其 leading 为漫画图片，title 为漫画标题的 Text，并且在被点击时显示一个仅用于该漫画的新屏幕，以更大格式显示漫画以及其 alt 文本。

我们稍后再担心构建漫画页面的问题：现在我们需要弄清楚如何显示该屏幕。我们之前在《Push/Pop Navigation Using the Navigator》中讨论过 push/pop 导航，这就是我们将要使用的：

```
@override
Widget build(BuildContext context) {
  return ListTile(
    leading: Image.network(
      comic["img"],
      height: 30,
      width: 30
    ),
    title: Text(comic["title"]),
    onTap: () {
      Navigator.push(
        context,
        MaterialPageRoute(
          builder: (BuildContext context) => ComicPage(comic)
        ),
      );
    }
```

```
},
});
}
```

- 在本节中我们定义了 ListTile 的 leading 小部件：一个从网络 URL 获取的 Image，即漫画的 ‘img’ 属性。
- 我们将图片预览的最大宽度和高度设置为 30 像素，以避免预览在列表中占用过多的垂直或水平空间。
- 这是每个 tile 将显示的文本。在本例中我们使用漫画的标题，即来自漫画 title 属性。
- 在这里我们设置了用户点击 tile 时会发生什么。在本例中，它使用我们在《Push/Pop Navigation Using the Navigator》中讨论的 push 操作打开一个 ComicPage（我们稍后会定义）。

完整的 ComicTile 类定义是：

```
networking_starting/xkcd_app/lib/main.dart

class ComicTile extends StatelessWidget {
ComicTile(this.comic);

final Map<String, dynamic> comic;

@override
Widget build(BuildContext context) {
return ListTile(
leading: Image.network(
comic["img"],
height: 30,
width: 30
),
```

```
title: Text(comic["title"]),
onTap: () {
Navigator.push(
context,
MaterialPageRoute(
builder: (BuildContext context) =>
ComicPage(comic)
),
);
},
```

```
});
}
}
```

## Building the Comic Page

ComicPage 将是我们到目前为止讨论过的最简单的 widget：我们将为它提供一个显示漫画编号的 AppBar，而 body 只是简单地是一个可滚动的 ListView，显示漫画的标题、图片和 alt 文本：

```
class ComicPage extends StatelessWidget {
  ComicPage(this.comic);

  final Map<String, dynamic> comic;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("#${comic["num"]}")),
      body: ListView(children: <Widget>[
        Center(
          child: Text(
            comic["title"],
            style: Theme.of(context).textTheme.display3,
          ),
        ),
        Image.network(comic["img"]),
        Padding(
          padding: EdgeInsets.all(8.0),
          child: Text(comic["alt"])
        ),
      ]),
    );
  }
}
```

所有这些加起来就是以下 `main.dart`：

```
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

import 'dart:async';
import 'dart:convert';

Future<int> getLatestComicNumber() async =>
  json.decode(
    await http.read('https://xkcd.com/info.0.json')
  )
  ["num"];
```

