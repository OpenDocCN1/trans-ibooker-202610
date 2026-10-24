

- ① **build** 方法只会被调用一次，因此无论将此调用放在何处都不会改变任何东西，但这就是你应该这样做的方式：将返回 **Future** 的调用放在只会被调用一次的地方，然后使用该调用返回的值作为 **FutureBuilder** 构造函数的 **future** 参数。

- ② 我们需要检查是否有任何收藏的漫画。如果有，我们将显示一个 **ListView** 的 **ComicTitles**，就像我们在主页中所做的那样。

- ③ 如果没有收藏的漫画，我们就只向用户显示一个通知。我们正在使用多行字符串，如本书记载的 Dart 附录中解释的那样（*Characters and Strings*）。

我们所看到的整个 **StarredPage** 代码被合并到以下类定义中：

```dart
// networking/xkcd_app/lib/main.dart

class StarredPage extends StatelessWidget {

  StarredPage();

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
        await http.read('https://xkcd.com/$n/info.0.json')
      );

      File('${dir.path}/$n.png')
        .writeAsBytesSync(await http.readBytes(comic["img"]));
      comic["img"] = '${dir.path}/$n.png';
      comicFile.writeAsString(json.encode(comic));

      return comic;
    }
  }
}
```

```dart
Future<List<Map<String, dynamic>>> _retrieveSavedComics() async {
  Directory docsDir = await getApplicationDocumentsDirectory();
  File file = File("${docsDir.path}/starred");
  List<Map<String, dynamic>> comics = [];

  if (!file.existsSync()) {
    file.createSync();
    file.writeAsStringSync('[]');
  } else {
    json.decode(file.readAsStringSync()).forEach(
      (n) async =>
        comics.add(await _fetchComic(n.toString()))
    );
  }
  return comics;
}

@override
Widget build(BuildContext context) {
  var comics = _retrieveSavedComics();

  return Scaffold(
    appBar: AppBar(
      title: Text("Browse your Favorite Comics")
    ),
    body:
    FutureBuilder(
      future: comics,
      builder: (context, snapshot) =>
        snapshot.hasData && snapshot.data.isNotEmpty ?
        ListView.builder(
          itemCount: snapshot.data.length,
          itemBuilder: (context, i) =>
            ComicTile(comic: snapshot.data[i]),
        )
        :
        Column(
          children: [
            Icon(Icons.not_interested),
            Text("""
              You haven't starred any comics yet.
              Check back after you have found something worthy of being here.
            """),
          ]
        )
      );
    }
  }
}
```

这就是我们的收藏漫画列表的显示效果：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_258_0.png)

## 为 HomeScreen 的 App Bar 添加链接

现在我们需要让用户能够从应用的其他部分访问此页面。我们将通过向 HomeScreen 添加一个 AppBar 操作来实现这一点。这将与我们之前添加的 app bar 操作非常相似：当用户点击图标时，我们会在当前视图之上显示 StarredPage：

```dart
// networking/xkcd_app/lib/main.dart

IconButton(
  icon: Icon(Icons.star),
  tooltip: "Browse Starred Comics",
  onPressed: () {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (BuildContext context) =>
          StarredPage()
      ),
    );
  },
),
```

这就是 HomeScreen 的 app bar 的显示效果：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_258_1.png)

## 我们接下来要去哪里

在本章中，我们通过构建一个 XKCD 漫画浏览器应用了解了如何与网络和本地存储进行交互。在下一章中，我们将探讨如何测试 Flutter 应用。

## 脚注

- [33] https://xkcd.com/2050/
- [34] https://api.dartlang.org/stable/2.1.0/dart-convert/json-constant.html
- [35] https://api.dartlang.org/stable/2.1.0/dart-convert/JsonCodec-class.html
- [36] https://pub.dartlang.org/packages/http
- [37] https://pub.dartlang.org/documentation/http/latest/http/http-library.html
- [38] https://en.wikipedia.org/wiki/List_of_HTTP_header_fields
- [39] https://material.io/design/components/navigation-drawer.html
- [40] https://material.io/design/components/progress-indicators.html#circular-progress-indicators
- [41] https://docs.flutter.io/flutter/material/InkWell-class.html
- [42] https://docs.flutter.io/flutter/material/Ink-class.html

## 第 6 章：测试和调试 Flutter 应用

到目前为止，我们讨论了 Flutter 应用是如何构建的，以及框架提供的所有不同的 UI、I/O 和状态管理功能。当一切按计划进行，代码完全按照你认为的方式运行时，这一切都很不错，但事实并非总是如此，测试可以简化开发过程，尤其是在大型项目中，通过确保每段代码都按预期执行。

这并不是本章唯一要讨论的内容：当你的应用出现问题，而你正在编写的方法无能为力时，异常会确保你能够通知应用的其他部分出了问题，让调用函数决定如何处理该问题。当应用执行 I/O 操作时，这尤其必要，因为 I/O 操作作用于用户和/或开发者无法控制的事物之外，如果不满足执行操作所需的条件，这些操作迟早会失败。

### 测试

可以对 Flutter 应用应用多种自动测试：

- 单元测试，最常见的一种，测试代码的较小部分，应该构成你在应用上进行的大多数测试。
- 集成测试，测试集成的大块代码部分，在 Flutter 的情况下，在模拟器或物理设备上运行应用，以检查用户以特定方式与其交互时其行为是否如预期。
- 组件测试，测试 UI 元素，而无需在模拟器或设备上启动应用，而是仅跟踪屏幕上应该出现的所有内容，并以比集成测试中更简单、更快速的方式与之交互。

在本节中，我们将以本书前面构建的计算器应用为例（第 3 章，*构建计算器应用*），编写一些测试以确保应用的每个部分都能正常工作。然后我们将测试上一章构建的 XKCD 漫画浏览应用，以向你介绍 Flutter 中的模拟对象和依赖注入。

### DART 单元测试

单元测试非常简单：你运行一段代码（例如，类构造函数和几个成员方法），并检查返回值是否符合预期。

要执行单元测试，我们需要在 `pubspec.yaml` 的 `dev_dependencies` 中添加 `test` 包，这是我们列出仅在开发和调试/测试时需要的依赖项，而在用户设备上执行应用时不需要的依赖项的部分。

### 为计算器应用编写单元测试

让我们先看看 `flutter create` 创建的应用树：它包含一个名为 `test` 的目录。我们将在那里添加我们希望 Flutter 执行的任何测试。

在这里我们将创建一个名为 calculation_test.dart 的文件，在其中，如名称所示，我们将测试 Calculation 类，如果你还记得（或回顾 实现计算），它允许我们一次添加表达式的小片段（作为 Strings），然后获取结果作为 double 数字或表示表达式的 String。

可以进行多种测试：我们将测试它是否执行所有不同的操作并返回正确的结果，然后是否可以处理多个操作，然后是否遵守运算符优先级。此外，我们需要检查它返回的字符串值和数值。

我们将从导入所需的类开始编写测试。在这种情况下，我们需要导入 test 包和定义了 Calculation 类的 calculator.dart 文件：

```
layout/calculator/test/calculation_test.dart
```

```dart
import 'package:flutter_test/flutter_test.dart';
import 'package:calculator/calculator.dart';
```

然后我们调用 Calculation 构造函数以获取一个可操作的[content]

- ① **build** 方法只会被调用一次，所以无论把这个调用放在哪里都不会改变任何东西，但这就是你应该这样做的方式：把返回 **Future** 的调用放在只会被调用一次的地方，然后使用该调用返回的值作为 **FutureBuilder** 构造函数的 **future** 参数。

- ② 我们需要检查是否有任何收藏的漫画。如果有，我们会像在主页中那样显示一个 **ListView** 的 **ComicTitles**。

- ③ 如果没有收藏的漫画，我们就只向用户显示一个通知。我们正在使用多行字符串，如本书记载的 Dart 附录中解释的那样（*Characters and Strings*）。

我们所看到的整个 **StarredPage** 代码被合并到以下类定义中：

```dart
// networking/xkcd_app/lib/main.dart

class StarredPage extends StatelessWidget {

  StarredPage();

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
        await http.read('https://xkcd.com/$n/info.0.json')
      );

      File('${dir.path}/$n.png')
        .writeAsBytesSync(await http.readBytes(comic["img"]));
      comic["img"] = '${dir.path}/$n.png';
      comicFile.writeAsString(json.encode(comic));

      return comic;
    }
  }
}
```

```dart
Future<List<Map<String, dynamic>>> _retrieveSavedComics() async {
  Directory docsDir = await getApplicationDocumentsDirectory();
  File file = File("${docsDir.path}/starred");
  List<Map<String, dynamic>> comics = [];

  if (!file.existsSync()) {
    file.createSync();
    file.writeAsStringSync('[]');
  } else {
    json.decode(file.readAsStringSync()).forEach(
      (n) async =>
        comics.add(await _fetchComic(n.toString()))
    );
  }
  return comics;
}

@override
Widget build(BuildContext context) {
  var comics = _retrieveSavedComics();

  return Scaffold(
    appBar: AppBar(
      title: Text("Browse your Favorite Comics")
    ),
    body:
    FutureBuilder(
      future: comics,
      builder: (context, snapshot) =>
        snapshot.hasData && snapshot.data.isNotEmpty ?
        ListView.builder(
          itemCount: snapshot.data.length,
          itemBuilder: (context, i) =>
            ComicTile(comic: snapshot.data[i]),
        )
        :
        Column(
          children: [
            Icon(Icons.not_interested),
            Text("""
              You haven't starred any comics yet.
              Check back after you have found something worthy of being here.
            """),
          ]
        )
      );
    }
  }
}
```

这就是我们的收藏漫画列表的显示效果：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_258_0.png)

## 为 HomeScreen 的 App Bar 添加链接

现在我们需要让用户能够从应用的其他部分到达此页面。我们将通过向 HomeScreen 添加一个 AppBar 操作来实现这一点。这将与我们之前添加的 app bar 操作非常相似：当用户点击图标时，我们会在当前视图之上显示 StarredPage：

```dart
// networking/xkcd_app/lib/main.dart

IconButton(
  icon: Icon(Icons.star),
  tooltip: "Browse Starred Comics",
  onPressed: () {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (BuildContext context) =>
          StarredPage()
      ),
    );
  },
),
```

这就是 HomeScreen 的 app bar 的显示效果：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_258_1.png)

## 我们接下来要去哪里

在本章中，我们通过构建一个 XKCD 漫画浏览器应用了解了如何与网络和本地存储进行交互。在下一章中，我们将探索如何测试 Flutter 应用。

## 脚注

- [33] https://xkcd.com/2050/
- [34] https://api.dartlang.org/stable/2.1.0/dart-convert/json-constant.html
- [35] https://api.dartlang.org/stable/2.1.0/dart-convert/JsonCodec-class.html
- [36] https://pub.dartlang.org/packages/http
- [37] https://pub.dartlang.org/documentation/http/latest/http/http-library.html
- [38] https://en.wikipedia.org/wiki/List_of_HTTP_header_fields
- [39] https://material.io/design/components/navigation-drawer.html
- [40] https://material.io/design/components/progress-indicators.html#circular-progress-indicators
- [41] https://docs.flutter.io/flutter/material/InkWell-class.html
- [42] https://docs.flutter.io/flutter/material/Ink-class.html

## 第 6 章：测试和调试 Flutter 应用

到目前为止，我们讨论了 Flutter 应用是如何构建的，以及框架提供的所有不同的 UI、I/O 和状态管理功能。当一切都按计划进行，代码完全按照你认为的方式运行时，这一切都很不错，但事实并非总是如此，测试可以简化开发过程，尤其是在大型项目中，通过确保每段代码都按其应有的方式执行。

这并不是我们在本章中要谈论的唯一内容：当你的应用出现问题，而你正在编写的方法无能为力时，异常会确保你能够通知应用的其他部分出了问题，让调用函数决定如何处理该问题。这在应用执行 I/O 操作时尤其必要，因为 I/O 操作作用于用户和/或开发者无法控制的事物之外，如果不满足执行操作所需的条件，这些操作迟早会失败。

### 测试

可以对 Flutter 应用应用多种自动测试：

- 单元测试，最常见的一种，测试代码的较小部分，应该构成你在应用上进行的大多数测试。
- 集成测试，测试集成的大块代码部分，在 Flutter 的情况下，在模拟器或物理设备上运行应用，以检查用户以特定方式与其交互时其行为是否如预期。
- 组件测试，测试 UI 元素，而无需在模拟器或设备上启动应用，而是仅跟踪屏幕上应该出现的所有内容，并以比集成测试中更简单、更快速的方式与之交互。

在本节中，我们将以本书前面构建的计算器应用为例（第 3 章，*构建计算器应用*）并编写一些测试，以确保应用的每个部分都能正常工作。然后我们将测试上一章构建的 XKCD 漫画浏览应用，以向你介绍 Flutter 中的模拟对象和依赖注入。

### DART 单元测试

单元测试非常简单：你运行一段代码（例如，类构造函数和几个成员方法）并检查返回值是否符合预期。

要执行单元测试，我们需要在 `pubspec.yaml` 的 `dev_dependencies` 中添加 `test` 包，这是我们列出仅在开发和调试/测试时需要的依赖项，而在用户设备上执行应用时不需要的依赖项的部分。

### 为计算器应用编写单元测试

让我们先看看 `flutter create` 创建的应用树：它包含一个名为 `test` 的目录。我们将在那里添加我们希望 Flutter 执行的任何测试。

在这里我们将创建一个名为 calculation_test.dart 的文件，在其中，如名称所示，我们将测试 Calculation 类，如果你还记得（或回顾 实现计算），它允许我们一次添加表达式的小片段（作为 Strings），然后获取结果作为 double 数字或表示表达式的 String。

可以进行多种测试：我们将测试它是否执行所有不同的操作并返回正确的结果，然后是否可以处理多个操作，然后是否尊重运算符优先级。此外，我们需要检查它返回的字符串值和数值。

我们将从导入所需的类开始编写测试。在这种情况下，我们需要导入 test 包和定义了 Calculation 类的 calculator.dart 文件：

```
layout/calculator/test/calculation_test.dart
```

```dart
import 'package:flutter_test/flutter_test.dart';
import 'package:calculator/calculator.dart';
```

然后我们调用 Calculation 构造函数以获取一个可操作的对象：

```
layout/calculator/test/calculation_test.dart
```

```dart
var calc = Calculation();
```

所有其他内容都将进入主函数：

```
layout/calculator/test/calculation_test.dart
```

```dart
void main() {
}
```

第一件事是定义一个 setUp 函数，该函数将在每个测试之前被调用，通过调用构造函数创建一个全新的计算对象，以确保每个测试都在隔离中运行。

