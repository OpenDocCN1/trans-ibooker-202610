

用户从 _controller.text 请求的漫画编号。Scaffold 的整个主体最终如下：

```
testing/xkcd_app/lib/main.dart
```

```
body: Column(
  mainAxisAlignment: MainAxisAlignment.center,
  children: [
    TextField(
      controller: _controller,
      decoration: InputDecoration(
        labelText: "Insert comic #",
      ),
      keyboardType: TextInputType.number,
      autofocus: true,
      onSubmitted: (String a) => Navigator.push(
        context,
        MaterialPageRoute(
          builder: (context) => FutureBuilder(
            future: fetchComic(a),

```

```
builder: (context, snapshot) {
      if(snapshot.hasError)
        return ErrorPage();
      if(snapshot.hasData)
        return ComicPage(snapshot.data);
      return CircularProgressIndicator();
    }
  ),
),
    ),
    key: Key("insert comic")
  ),
  FlatButton(
    key: Key("submit comic"),
    child: Text("Open".toUpperCase()),
    onPressed: () => Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => FutureBuilder(
          future: fetchComic(_controller.text),
          builder: (context, snapshot) {
            if(snapshot.hasError)
              return ErrorPage();
            if(snapshot.hasData)
              return ComicPage(snapshot.data);
            return CircularProgressIndicator();
          }
        ),
      ),
    )
  )
],
),
```

## 使用 flutter_driver 测试选择页面

集成测试需要一个名为 flutter_driver 的包。
包含测试 Flutter 应用所需全部依赖（以及我们运行 XKCD 应用所需的其他依赖）的完整 pubspec.yaml 文件如下：

```
testing/xkcd_app/pubspec.yaml
```

```
name: xkcd_app
description: An app to display XKCD comics.

dependencies:
  http: ^0.12.0+1
  path_provider:
  url_launcher: ^5.0.2

  flutter:
    sdk: flutter
```

```
dev_dependencies:
  flutter_driver:
    sdk: flutter
  mockito:
  test:

  flutter:
    uses-material-design: true
```

要使用它，让我们在应用源码树的根目录下创建一个名为 `test_driver` 的新目录。在其中，我们需要创建一个文件来启用 Flutter driver 扩展运行应用，以及另一个文件来连接该运行中的应用并对其执行一些测试。

作为示例，我们将探讨如何与 `SelectionPage` 交互，以及了解使用 Flutter 集成测试框架可以做什么。

### 创建 selectionpage.dart 文件

让我们从创建一个启用 Flutter driver 扩展的文件开始：

```
testing/xkcd_app/test_driver/selectionpage.dart
```

```dart
import 'package:flutter_driver/driver_extension.dart';
import 'package:xkcd_app/main.dart' as app;
import 'package:flutter/material.dart';

void main() {
  enableFlutterDriverExtension();

  runApp(
    MaterialApp(
      home: app.SelectionPage(),

```
)
);
}
```

① *package:flutter_driver/driver_extension.dart* 是 *flutter_driver* 的一部分，它允许我们启用 *driver 扩展*，在运行我们想要在其上运行集成测试的应用时，需要启用这些扩展。

我们还需要导入应用的 main.dart，因为我们需要能够运行应用，并且我们需要导入 Flutter 的 Material Design 库，因为在这种情况下我们只想运行 SelectionPage，所以我们需要访问 runApp 和 MaterialApp 类。

① enableFlutterDriverExtension 实际上启用 driver 扩展。此函数包含在 *package:flutter_driver/driver_extension.dart* 中。

① 我们想要测试选择页面，因此我们就像构建 Flutter 应用时运行常规应用主屏幕一样运行它，但改为选择 SelectionPage。

这是唯一一个能够访问你的应用或 Flutter UI 类的文件。实际的测试文件绝不能导入其中任何内容。

### 创建 selectionpage_test.dart 文件

此文件名需要与上一个文件名相同，但在 .dart 扩展名之前加上 _test。在本例中，它必须命名为 selectionpage_test.dart。

这个测试将非常简单，只是尝试打开一个漫画并检查应用是否正确打开了它：

```
testing/xkcd_app/test_driver/selectionpage_test.dart
```

① import 'package:flutter_driver/flutter_driver.dart';
import 'package:test/test.dart';

void main() {

```
group('Selection Page', () {
  FlutterDriver driver;
  SerializableFinder appBarText = find.byValueKey("AppBar Text");

  setUpAll(() async {
    driver = await FlutterDriver.connect();
  });

  tearDownAll(() async {
    if (driver != null) {
      driver.close();
    }
  });

  test('Verify Page is Loaded', () async {
    await driver.waitFor(appBarText);
    expect(await driver.getText(appBarText), "Comic selection");
  });

  test('Open Comic', () async {
    await driver.tap(find.byValueKey("insert comic"));
    await driver.enterText("1");
    await driver.tap(find.byValueKey("submit comic"));
    await driver.waitFor(find.text("#1"));
    expect(await driver.getText(appBarText), "#1");
  });
});
}
```

- ① 这里我们需要导入主要的 `flutter_driver.dart` 文件，以及常规的 test 包。
- ② 让我们定义一些我们会多次使用的变量：我们将用来与运行中的 Flutter 应用交互的 `FlutterDriver`，以及查找使用字符串 *AppBar text* 创建的 `Key` 的 `SerializableFinder`，你应该将那个 Key 添加到应用中的每个 `AppBar Text` 组件上。
- ③ 此部分用于在测试开始时连接到运行中的应用。
- ④ 测试结束时我们在这里断开与应用的连接。
- ⑤ 第一个测试非常简单：我们等待页面加载，然后检查 `AppBar` 文本是否正确（这样我们就知道应用加载了正确的页面）。
- ⑥ 这个测试更重要：我们找到用于输入漫画的 `TextField`，点击它（以选中它，以防万一），输入文本 *1*，按下新建的 *submit* 按钮，等待漫画页面加载，并检查漫画编号 1 是否确实已加载（AppBar 文本应为 #1）。

要开始测试，请在连接了模拟器或设备的情况下运行以下命令：

```
flutter drive --target=test_driver/selectionpage.dart
```

你会看到应用打开并自动执行你要求它做的所有事情。在命令行中，你会得到所有测试已通过的确认。

## 更多测试

在我们的集成测试示例中，我们只测试了 SelectionPage。这是因为它允许我们组合许多有趣的 flutter_driver 特性，而且不需要重复多次相同的指令，这对于一个旨在帮助你学习新事物的示例来说不会太有趣。

我们没有对计算器应用进行集成测试，但它可以用来测试使用键盘执行计算，同时测试计算和 UI，这不过是我们已经做过的测试的组合：它在真实应用中有用，但你已经知道如何去做。这就是集成测试的用途，就像我们为 XKCD 应用编写的测试一样：它同时测试 UI 和逻辑。

测试代码库至关重要，应该尽可能多地进行。在编写自己的应用时，重要的是你为应用的每个方面设计和编写全部三种测试：我们的 XKCD 应用需要针对 HomeScreen、ComicPage 进行测试，也许还需要测试在需要时是否显示 ErrorPage。此外，更多针对基本 UI 元素的单元测试和组件测试也很有用（例如，测试 HomeScreen 上的 AppBar 控件）。

这些测试用例的代码示例与之前的很相似，因此我没有在此复现它们。有一种方法可以让测试更容易，但不太可预测或可控，因为它依赖于本地机器的文件系统，如果测试在服务器上运行且不允许代码写入文件系统，则可能无法访问。

另外，一个即使在集成测试中也绝对需要 mock 对象的重要特性是身份验证：需要身份验证才能工作的应用会使测试无法进行，除非测试利用 mock 来绕过身份验证的问题。

## Mockito 与自建 Mock 对象对比

Mockito 并不总是让创建 mock 对象更容易：相反，有时你可能会发现直接编写一个实现所需方法的类更容易。你可以通过将这些类定义为子类（*extends*）或定义为实现（*编写一个实现接口的类*）你想要 mock 的接口的类来使它们工作。

