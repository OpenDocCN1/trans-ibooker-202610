
## 接下来做什么

在下一章中，我们将结合使用本章中看到的一些布局组件来构建一个计算器应用。

## 脚注

+   1. [21] https://material.io/design/components/dividers.html
+   2. [22] https://pragprog.com/favicon.png
+   3. [23] https://material.io/design/components/snackbars.html#anatomy
+   4. [24] https://material.io/design/components/cards.html
+   5. [25] https://material.io/design/components/chips.html

## 第 3 章

## 构建计算器应用

呼！你已经学到了很多 Flutter 中可用的工具。我们要把它们用起来吗？正如所承诺的，本章将专注于构建一个计算器应用，使用我们在上一章中看到的一些结构和元素。

我们将从展示如何从一个空文件到构建出一个可运行的 Flutter 应用开始。

这一次，使用 `flutter create appname` 命令或你选择的 IDE 创建一个 Flutter 项目，但这一次要删除生成的 `main.dart` 中的所有内容。

## 导入基本依赖

你应该始终做的第一件事就是导入依赖。

在这种情况下，我们只需要上一章中使用的同一个 Flutter Material 库：

layout/calculator/lib/main.dart

```
import "package:flutter/material.dart";
```

## 编写 main

让我们从应用中最简单、最不具体的部分开始：main 函数。

### 使用包装类

在上一章中，我们定义了一个 MyApp 类，并使用 main 简单地运行它，如下所示：

```
void main() =>
  runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      // Actual app content and data
    );
  }
}
```

### 跳过包装类

只需编写以下代码，就可以达到相同的效果：

```
void main() {
  runApp(MaterialApp(
    // other app content and data
  ));
}
```

这看起来简短很多，但在一个更大的应用中，相比之下其实并没有节省多少。

如果你的应用完全包含在一个视图中，这不会损害应用功能；但如果你需要更多视图并需要共享数据，就应该考虑为应用编写一个包装类，以保持内容组织有序、清晰明了。

而且，无论如何，只有在确实需要保持代码紧凑，或者为了节省时间快速编写一个小应用时，才应该使用第二种形式。

因此，我们将编写以下代码：

layout/calculator/lib/main.dart

```
void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "Flutter Calculator",
      theme: ThemeData(
        primarySwatch: Colors.blue,
        backgroundColor: Colors.black26
      ),
      home: CalculatorHomePage(title: "Flutter Calculator"),
    );
  }
}
```

## 计算器的主页

主页将是有状态的（stateful），因为它需要反映用户的交互和输入，所以我们将定义一个简单的 `StatefulWidget`：

```
layout/calculator/lib/main.dart

class CalculatorHomePage extends StatefulWidget {
  CalculatorHomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _CalculatorHomePageState createState() => _CalculatorHomePageState();
}
```

以及它的状态：

```
layout/calculator/lib/main.dart

class _CalculatorHomePageState extends State<CalculatorHomePage> {
}
```

![](img/eb93f09b5f47b8f6c9bead1b9047c506_121_0.png)

将 Flutter 应用表示为树是可视化 Flutter 应用布局的一种非常有效的方式。

在本例中，应用的主要结构将是一个包含三个子项的 `Column`：

+   - 计算器的屏幕，它将是一个带有一些 `Text` 的 `Card`。
+   - 一个 `Row`，包含删除整个表达式的按钮和一次删除一个字符的按钮，也就是几个 `FlatButtons`。
+   - 一个 `Row`，用于放置计算器的其余按钮，其中包含两个 `Column` 组件：一个用于数字所在的三个列（由四行组成，每行四个按钮），另一个用于运算符按钮所在的列。

为了让布局看起来更好、运行得更好，我们将大量使用 `Expanded`（无意双关），但这将在后面的 _使用 Expanded 让应用外观和功能都正确_ 中谈到。

## 实现基本布局

在 `_CalculatorHomePageState` 的花括号内，我们将定义一个带有常用 Scaffold 和 AppBar 的 build 方法：

```
layout/calculator_part1/lib/main.dart
```

```
@override
Widget build(BuildContext context) {
```

```
return Scaffold(
  appBar: AppBar(
    title: Text(widget.title),
  ),
```

```
);
}
```

Scaffold 的 body 将如下所示：

```
body: Column(
  crossAxisAlignment: CrossAxisAlignment.stretch,
  children: <Widget>[
    calculatorScreen,
    Row(
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: <Widget>[
      ],
    ),
    Row(
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: <Widget>[
        Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: <Widget>[
            Row(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: <Widget>[
              ],
            ),
            Row(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children:<Widget>[
              ],
            ),
          ],
        ),
        Row(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: <Widget>[
          ],
        ),
      ],
      Row(
```

```
crossAxisAlignment: CrossAxisAlignment.stretch,
children: <Widget>[
    ],
),
],
),
```

```
Column(
  crossAxisAlignment: CrossAxisAlignment.stretch,
  children: <Widget>[
    ],
  ),
],
),
],
),
```

CrossAxisAlignment 的这些选项旨在尽可能将组件拉伸开。

仅靠这一点无法实现的效果，将由 `Expanded` 完成（我们会在定义完布局元素之后再讨论 `Expanded`）。

## 添加删除行

第一个 `Row` 组件将包含用于删除单个字符的按钮和用于删除整个表达式的按钮。

应用中的所有按钮都将调用 `_CalculatorHomePageState` 的方法，所以我们需要定义这些方法。因此，直接在代码下方添加以下空方法定义，我们会在“在应用内部使用计算”中定义了处理计算的类之后，再来填充它们：

```
layout/calculator_part1/lib/main.dart

void add(String a) {
}

void deleteAll() {
}

void deleteOne() {
}

void getResult() {
}
```

回到应用布局定义，这些按钮将是 `FlatButton` 按钮。你需要通过在该 `Row` 的 `children` 属性的方括号内插入以下代码，将它们添加到你在代码中看到的第一个 `Row` 组件中：

```
layout/calculator_part1/lib/main.dart

FlatButton(
  child: Text(
    'C',
    style: TextStyle(color: Colors.white),
  ),
  onPressed: (){deleteAll();},
  color: Colors.black54,
),
FlatButton(
  child: Text(
    '<-',
    style: TextStyle(color: Colors.white)
  ),
  onPressed: (){deleteOne();},
  color: Colors.black87,
),
```

使用这段代码后，左侧按钮（全部删除）将变为灰色，右侧按钮（删除一个字符）将变为黑色。

这是因为颜色名称后面的数字（这种用法只适用于黑色和白色）指定了它们的透明度；数字越低表示越透明、几乎不可见，而数字越高则用于使对象非常不透明。

## 第三行

我们还有一行。这听起来可能有点奇怪，直到你回头看看此处图中应用的结构。这一行将有两个子项：

+   - 左侧是数字、小数点以及等号按钮。
+   - 右侧是运算符按钮。

### 添加数字按钮

数字按钮将分布在四行中：

+   - 第一行将包含数字 7、8 和 9。
+   - 第二行将包含数字 4、5 和 6。
+   - 第三行将包含数字 1、2 和 3。
+   - 第四行将包含数字 0、小数点以及用于计算结果的按钮。

这意味着，在这个 `Row` 内部，我们需要另一个 `Column`，并将四个 `Row` 作为其子项，仅用于显示数字按钮。这些按钮将采用深蓝色背景上的白色文本，但 `=` 按钮除外，它将在非常浅的蓝色背景上使用黑色文本。因此，作为我们应用第三行的第一个子项，我们将编写以下代码：

```
layout/calculator_part1/lib/main.dart

Column(
  crossAxisAlignment: CrossAxisAlignment.stretch,
  children: <Widget>[
    Row(
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: <Widget>[
        FlatButton(
          child: Text(
            '7',
            style: TextStyle(color: Colors.white),
          ),
          onPressed: () {add('7');},
          color: Colors.blueAccent,
        ),
        FlatButton(
          child: Text(
            '8',
            style: TextStyle(color: Colors.white),
          ),
          onPressed: () {add('8');},
```

