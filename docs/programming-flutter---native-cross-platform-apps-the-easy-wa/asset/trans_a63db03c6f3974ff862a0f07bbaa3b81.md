
可以使用 `MaterialBasedCupertinoThemeData(materialTheme: themeData)` 来完成，它接受一个名为 `materialTheme` 的命名参数，参数值是我们想要从中派生新的 `CupertinoThemeData` 的 `ThemeData`。

## 我们的 CupertinoApp

因此，`theme` 参数完全可以省略，除非你希望应用有一个深色背景，并配合 `activeOrange` 主色。我们将坚持使用带有蓝色主色的常规浅色主题，提供与原始 Material 示例中相同的参数，以使示例尽可能接近原始：

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return CupertinoApp(
      title: 'Flutter Demo',
      theme: CupertinoThemeData(
        primaryColor: CupertinoColors.activeBlue,
      ),
      home: MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

## 未更改的组件

`StatefulWidget`、`StatelessWidget` 和 `State` 与 Material 应用中的完全相同，`Row`、`Column`、`Container` 和 `Center` 组件也是如此，以及像 `Icons`（即使它们使用的 `IconData` 应该从 `CupertinoIcons` 中获取，而不是从 `Icons` 中获取）和 `Text` 这样的简单组件。

## Scaffold 和 App Bar

但在讨论它们之前，`Scaffold` 需要换成 `CupertinoPageScaffold`（对于带有底部导航的脚手架，有 `CupertinoTabScaffold`，没有 `CupertinoScaffold`），正如我之前所预料的，它接受 `CupertinoNavigationBar` 作为 `navigationBar` 参数，而不是像我们在 Material 应用中那样接受 `AppBar` 作为 `appBar` 参数。此外，`CupertinoNavigationBar` 接受三个命名特殊的参数：leading、middle 和 trailing，用于放置导航栏中的组件。通常左侧留给返回按钮，右侧留给重要操作，中间则放置页面标题：

```
cupertino/counter_cupertino/lib/main.dart
```

```
return CupertinoPageScaffold(
  navigationBar: CupertinoNavigationBar(
    middle: Text(widget.title),
  ),
```

## CupertinoButton

增加计数器的按钮将是一个——你猜对了——`CupertinoButton`。它接受一个子组件，我们可以使用一个简单的 `Row`：

```
cupertino/counter_cupertino/lib/main.dart
```

```
CupertinoButton(
  onPressed: _incrementCounter,
  child: Row(
    mainAxisAlignment: MainAxisAlignment.center,
    children: [
      Icon(CupertinoIcons.add_circled_solid),
      Text("Increase")
    ]
  )
)
```

`CupertinoIcons.add` 是单独出现时应该使用的 + 图标，`CupertinoIcons.add_circled` 周围有一个圆圈，`CupertinoIcons.circled_solid` 则是你之前截图中的那个。

## 总结

完整代码，包括与我们在第一章看到的应用相同的部分，如下所示：

```
cupertino/counter_cupertino/lib/main.dart
```

```
import 'package:flutter/cupertino.dart';

void main() => runApp(MyApp());
```

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return CupertinoApp(
      title: 'Flutter Demo',
```

## 其他 Cupertino 组件

这只是对如何构建 iOS 风格 UI 的快速介绍，旨在作为入门平台，而非全面指南，因此我们将只讨论最重要的 Cupertino 组件。你可以在 Flutter 官网的专门章节中找到更多相关细节。[^56]

## Cupertino 导航

在讨论一些需要转换为 Cupertino 组件的 UI 元素之前，我们将了解如何使用 Cupertino 组件实现导航。Material Design 和 iOS 布局之间有一些差异需要澄清：抽屉式侧边导航菜单是典型的 Material Design 元素，因此它没有对应的 Cupertino 实现，导航要么通过底部标签栏处理，要么通过简单的 push/pop 导航处理。

## 使用 CupertinoTabScaffold 实现基于标签页的导航

让我们来谈谈上一节中简要提到的内容：`CupertinoTabScaffold`。实际上，要实现基于标签页的导航，这个组件并不是必需的（因为可以使用 `CupertinoTabBar` 调用 `setState` 并根据所选标签页渲染不同数据），但它让这件事变得非常简单。

它接受两个参数：

- `tabBar`，必须设置为一个 `CupertinoTabBar`，其中包含所需的 `BottomNavigationBarItems` 列表。
- `tabBuilder`，在给定 `context` 和 `int` 索引时，必须返回与该索引对应的标签页内容，通常返回一个 `CupertinoTabView`，它为每个标签页提供自己的导航历史和状态，并包含一个 `CupertinoPageScaffold` 用于标签页内容。

下面是一个示例：

```
class HomePage extends StatelessWidget {

  final data = [
    {
      "Train",
      CupertinoIcons.train_style_one
```

```
},
{
  "Search",
  CupertinoIcons.search
},
{
  "Share",
  CupertinoIcons.share
}
];

@override
Widget build(context) =>
CupertinoTabScaffold(
  tabBar: CupertinoTabBar(
    items: data.map(
      (el) =>
        BottomNavigationBarItem(
          title: Text(el.first),
          icon: Icon(el.last)
        )
    ).toList(),
  ),
  tabBuilder: (context, i) =>
    CupertinoTabView(
      builder: (context) =>
        CupertinoPageScaffold(
          navigationBar: CupertinoNavigationBar(
            middle: Text(data[i].first),
          ),
          child: Center(
            child: Icon(data[i].last, size: 80.0)
          ),
        )
    );
}
```

打开后会产生以下视图：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_401_0.png)

并且它让我们有机会注意到 iOS 的共享图标，它与 Material 的共享图标非常不同，在点击第三个标签页时可以看到：

## 使用 CupertinoPageRoute 进行 Push/Pop 导航

`Navigator` 本身的工作方式与 Material 应用中完全相同，但我们需要将 `MaterialPageRoute` 替换为 `CupertinoPageRoute`，不过并非必须如此。`MaterialPageRoute` 实际上会适配页面间转换时常用的原生动画（Android 上为垂直，iOS 上为水平），但它包含在 material.dart 中。如果你希望在 Android 上使用 iOS 动画（`CupertinoPageTransition`），或者因为当前文件只导入了 cupertino.dart 而不想使用 material.dart 中的组件，你可以像使用 `MaterialPageRoute` 一样使用 `CupertinoPageRoute`，将它传递给 `Navigator.push` 或类似方法，并向 `CupertinoPageRoute` 构造函数传入通常的 `context` 和一个 builder 回调。

在 push 一个页面后，导航栏的 leading 组件（如果存在）将是一个按钮，允许用户返回到上一个页面。

![](img/eb93f09b5f47b8f6c9bead1b9047c506_402_0.png)

## 一些 UI 元素的替代

有一些 UI 元素值得讨论，作为我们在本书中见过的 Material 组件的替代品。

## CupertinoTextField

需要更改的组件之一是 `TextField`，与其他一些 Cupertino 组件不同，它不仅仅需要改名：`CupertinoTextField` 的 `decoration` 类型是 `BoxDecoration`（与我们在 `Container` 中使用的一样），而不是 `InputDecoration`。`InputDecoration` 是我们用来提供 `labelText` 的，而现在它变成了 `CupertinoTextField` 自身的一个名为 `placeholder` 的参数：

```
TextField(
  controller: _controller,
  decoration: InputDecoration(
    labelText: "Description"
  ),
  onSubmitted: (val) {
    // do something with value
  }
)
```

将变成：

```
CupertinoTextField(
  controller: _controller,
  placeholder: "Description",
  onSubmitted: (val) {
    // do something with value
  }
)
```

## CupertinoActivityIndicator

`CircularProgressIndicator` 的替代品是 `CupertinoActivityIndicator`，它看起来像这样：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_403_0.png)

## CupertinoAlertDialog

Cupertino 库提供了 `CupertinoAlertDialog`，通过 `showCupertinoDialog` 启动，以替代使用 `showDialog` 启动的 Material `AlertDialog`。

