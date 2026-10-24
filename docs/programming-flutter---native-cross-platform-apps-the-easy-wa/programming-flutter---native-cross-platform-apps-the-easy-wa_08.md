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

### 为 HomeScreen 的 App Bar 添加链接

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

### 我们接下来要去哪里

在本章中，我们通过构建一个 XKCD 漫画浏览器应用了解了如何与网络和本地存储进行交互。在下一章中，我们将探索如何测试 Flutter 应用。

### 脚注

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





```
```dart
setUp(() {
  calc = Calculation();
});
```
```

我们将调用测试函数，该函数接受一个字符串和一个回调函数作为位置参数。字符串是对测试的文本描述，以便我们了解哪些测试失败了（如果有的话），而回调则是在该测试触发时要运行的代码。在回调内部，我们将使用 `expect` 函数来指定某个指令应该返回什么值。例如，如果我们想测试 `Calculation` 类中的简单加法操作，可以编写如下代码：

```
```dart
test('simple addition', () {
  calc.add("5");
  calc.add("+");
  calc.add("6");
  calc.add("5");
  expect(calc.getResult(), 70.0);
});
```
```

如果想在同一个测试中测试多个加法/减法操作，只需在其后添加以下代码：

```
```dart
test('more sums', () {
  calc.add("5");
  calc.add("5");
  calc.add("-");
  calc.add("5");
  calc.add("+");
  calc.add("50");
  expect(calc.getResult(), 100.0);
});
```
```

我们还需要测试乘法是否正常工作，如下所示：

```
```dart
test('simple multiplication', () {
  calc.add("5");
  calc.add("x");
  calc.add("6");
  expect(calc.getResult(), 30.0);
});
```
```

```
});
```

除法也很重要，我们还有机会检查是否可以正确返回包含小数部分的浮点值：

layout/calculator/test/calculation_test.dart

```
test('division', () {
  calc.add("5");
  calc.add("/=");
  calc.add("2");
  expect(calc.getResult(), 2.5);
});
```

运算优先级不能想当然，因此我们需要检查是否被正确遵循：

layout/calculator/test/calculation_test.dart

```
test('precedence', () {
  calc.add("5");
  calc.add("+-");
  calc.add("6");
  calc.add("x");
  calc.add("5");
  expect(calc.getResult(), 35.0);
});
```

最后要检查的是 `getString` 操作是否正确工作：

layout/calculator/test/calculation_test.dart

```
test('string', () {
  calc.add("5");
  calc.add("x");
  calc.add("6");
  calc.add("+-");
  calc.add("7");
  expect(calc.getString().toString(), "5x6+7");
});
```

整个 calculation_test.dart 文件的内容如下：

layout/calculator/test/calculation_test.dart

```
import 'package:flutter_test/flutter_test.dart';
import 'package:calculator/calculator.dart';

var calc = Calculation();

void main() {
  setUp(() {
```

```
```javascript
calc = Calculation();
});
test('simple addition', () {
  calc.add("5");
  calc.add("+");
  calc.add("6");
  calc.add("5");
  expect(calc.getResult(), 70.0);
});
test('more sums', () {
  calc.add("5");
  calc.add("5");
  calc.add("-");
  calc.add("5");
  calc.add("+");
  calc.add("50");
  expect(calc.getResult(), 100.0);
});
test('simple multiplication', () {
  calc.add("5");
  calc.add("x");
  calc.add("6");
  expect(calc.getResult(), 30.0);
});
test('division', () {
  calc.add("5");
  calc.add("÷");
  calc.add("2");
  expect(calc.getResult(), 2.5);
});
test('precedence', () {
  calc.add("5");
  calc.add("+");
  calc.add("6");
  calc.add("x");
  calc.add("5");
  expect(calc.getResult(), 35.0);
});
test('string', () {
  calc.add("5");
  calc.add("x");
  calc.add("6");
  calc.add("+");
  calc.add("7");
  expect(calc.getString().toString(), "5x6+7");
});
}
```
```

### 进行自定义修改

如果你在计算器应用中实现了括号功能，也应该考虑为其编写一个单元测试。

编写单元测试非常简单：实际上，你只需要在想要添加运算符时拆分 `calc.add` 调用即可。不过，建议在至少部分测试中逐个数字添加数字，因为这是实际计算器应用的运行方式，而且可能存在仅在这种操作方式下才会显现的问题（也许一次添加多位数字会破坏在真实计算器中能正常工作但在我们实现中会出错的逻辑，但就我们的实现而言情况并非如此）。

在此阶段，每次尝试构建你的应用时，这些测试都会运行。你也可以在命令行使用 `flutter test` 来运行测试。然而，使用 Visual Studio Code 运行测试可能是最好的方式，它为每个测试显示一个运行按钮，允许你运行每个测试并在直观的 UI 中查看结果。Android Studio 也在行号旁边显示一个向右的箭头按钮来运行测试，但我发现其 UI 略显缺乏直观性和一致性。不过，你的感受可能不同，尤其是如果你更喜欢 Android Studio/IntelliJ 的 UI 而不是 Visual Studio 的 UI；请选择你更习惯使用的工具。

### FLUTTER 小部件和集成测试

单元测试足够简单直接，但小部件测试和集成测试需要更多关注，因为它们依赖于 UI，而 UI 并不像计算处理代码那样枯燥和简单。

我们应用中的每个低级逻辑片段可能是正确的，但 UI 或某些已测试代码片段的组合中可能存在错误。这就是小部件测试和集成测试存在的原因。它们用于测试单个小部件或应用的更大部分是否正常工作。

### Flutter 小部件测试

Flutter 提供了一种测试小部件是否正常工作以及以编程方式与应用中的小部件进行交互的方法。

如果你不习惯类似的测试基础设施，这可能很难想象，因此这里给出一个示例，说明在导入 main.dart 后，如何测试按下 5 按钮是否真的会让数字 5 出现在计算器屏幕上：

```
layout/calculator/test/widget_test.dart
```

```
testWidgets('5 press test', (WidgetTester tester) async {
  await tester.pumpWidget(new MyApp());

  expect(find.text('5'), findsOneWidget);

  await tester.tap(find.text('5'));
  await tester.pump();
```

```
⑥ expect(find.text('5'), findsNWidgets(2));
});
```

+   ① `testWidgets` 函数与 `test` 函数非常相似，但有一个重大区别：它向其回调传递一个 `WidgetTester` 对象，该对象正是使小部件测试变得特殊的地方。

+   ② *pump* 意味着 *构建* 或 *渲染*。`tester.pumpWidget` 会渲染我们已有定义的小部件。

需要注意的是，尽管 `pumpWidgets` 给人的印象是它可以 pump 任何类型的小部件，但如果你的小部件中有 `Scaffold`，它需要被包裹在 `MaterialApp` 中（或者更通用的 `WidgetsApp` 类，后者不依赖 Material Design 类），因为它需要在作用域内有一个 `MediaQuery` 小部件，而上述类提供了这样的小部件。

+   ③ 这是第一个 `expect` 调用。当我们第一次构建应用时，应该只有一个包含字符串 `5` 的小部件存在（即 5 按钮）。

`find` 是一个常量。我们稍后会在本节的后半部分详细探讨其用法。

`find.text` 是一个 Finder，而 `findsOneWidget` 是由 `flutter_test` 提供的 Matcher 常量。

在本代码解释之后的两段中，我们将详细列出所有的 Finders 和 Matchers。
你现在只需要知道 Matcher 用于判断 Finder 提供的结果，而 Finder 是根据特定特征查找小部件的。

+   ④ 这相当自解释：我们点击该小部件。

+   ⑤ 在测试期间，Flutter 小部件在调用 `setState` 时不会自动重新构建：为此，我们需要调用 `tester.pump`，它会重新渲染应用等待渲染的下一帧。

还有一个名为 `tester.pumpAndSettle` 的方法，它会持续渲染帧，直到没有等待重新渲染。这在某些操作触发需要多帧完成的动画时很有用。

这与其他 `find.text` 检查一样，但这次应该有两个具有该文本的小部件：一个是按钮，另一个是屏幕，屏幕现在应该显示该数字。

正如你所见，界面相当灵活且直观，对于检查 UI 是否实际正确工作非常有用。

### 所有的 Finders

让我们更全面地了解 Flutter 小部件测试：使用 `find` 常量查找小部件有多种方式，该常量是一个集合（类 CommonFinders[43]），包含最常用的 Finders：

- `find.text(text)`，查找显示与其 text 参数相等的字符串的 Text 小部件。
- `find.byIcon(icon)`，查找显示与其 icon 参数相等的 IconData（例如 Icons.not_interested）的 Icon 小部件。
- `find.byKey(key)`，回到 The Key 概念：它匹配你作为参数提供的具有该 Key 的小部件。
- `find.byElementPredicate(predicate)`，这非常灵活：predicate 参数（类型为 WidgetPredicate）只是一个回调，它接收一个 Widget 作为参数，返回 `true` 或 `false` 将分别被视为正在检查的小部件满足或不满足包含在 Finder 结果中的要求，这比其他 Finders 给开发者提供了决定匹配条件的更大自由度。
- `find.byWidget(widget)`，这是最具体的 Finder：它仅匹配作为参数给出的 Widget。

`find` 常量提供了其他不太常见的 Finder：

- `find.byTooltip`;
- `find.byElementType`;
- `find.byType`;
- `find.widgetWithIcon`;





- find.widgetWithText;
- find.byDescendant.

### 后台控件（Offstage Widgets）

我们之前介绍的所有 Finders 都允许你设置一个名为 `skipOffstage` 的布尔型参数。如果将其设置为 `true`，则任何包含在很少使用的 **Offstage** 控件中的 widget 都会被跳过。Offstage 是一个类，它允许你将某些 widget（作为 Offstage 的子级）进行布局并占用空间，而不实际显示它们。

### 所有匹配器（Matchers）

除了 `findsNothing`、`findsOneWidget` 和 `findsNWidgets(n)` 之外，还有 `findsWidgets`，它是 `findsNothing` 的反义（等同于 `!findsNothing`）。

### 更多计算器测试

让我们来看看一个更完整、更现实的 widget 测试示例，针对我们的计算器应用进行测试。

### 检查网格

5 次按键测试的第一部分实际上可以转化为一个非常实用的功能：检查整个网格是否确实被渲染，以及用户是否为每个按钮都有一个对应的按钮。除了除号按钮外，数字、删除按钮、. 按钮以及所有操作按钮都包含一个 `Text` 来显示其用途。这意味着我们可以使用 `find.text` 来匹配它们：

```
layout/calculator/test/widget_test.dart

testWidgets('grid existence test', (WidgetTester tester) async {
  await tester.pumpWidget(new MyApp());

  expect(find.text('0'), findsNWidgets(2));

  for(int i = 1; i < 10; i++) {
    expect(find.text('$i'), findsOneWidget);
  }

  ['+', '-', 'x', '<-', 'C'].forEach(
    (str) => expect(find.text(str), findsOneWidget)
  );

  expect(find.byElementPredicate((element) {
    if(!(element.widget is Image))
      return false;
    else if(
```
```
```dart
(element.widget as Image).image
==
AssetImage("icons/divide.png")
)
return true;
else return false;
}), findsOneWidget);

});
```
```

- ① 应该有两个包含 *0* 字符串的 widget：计算器屏幕在开始时显示的 0，以及零按钮。
- ② 从 1 到 10 的数字可以通过简单地使用 `for` 循环找到。
- ③ 我们在本书中很少使用 `forEach`，但它非常有用：它对 `List` 或 `Iterable` 的每个成员执行相同的回调。在这种情况下，它是 for 循环遍历符号列表的替代方案，或者仅仅是单独编写查找每个符号指令的替代方案。
- ④ 显然，我们不能对图像使用 `find.text`，而 `find.byWidget` 可能是一个可行的替代方案。然而，问题在于它在我们这种情况下并不起作用：它寻找的是*完全*匹配，如果我们尝试查找，它不会找到我们的图像，因为生成的对象通常包含控制 widget 位置和其他我们创建 widget 时并未直接设置的属性的额外参数。

这时 `find.byElementPredicate` 就派上用场了：我们可以自己判断 widget 是否匹配我们想要的条件，这意味着我们可以专注于我们需要的部分：我们需要该 widget 是一个图像，并且希望它显示来自 assets 的 `icons/divide.png`。

传递给回调闭包的是一个 `Element`（它是 widget 的一个实例，并包含其在 widget 树中位置的一些上下文信息，这在我们当前做的事情中并不需要）。我们可以访问该 `Element` 衍生自的 `Widget`。

通过访问其 **widget** 成员属性，因此当使用 `element.widget` 时，我们就是在执行此操作。

如果该 widget 不是 **Image** 的实例（这就是 `is` 的用途，它检查该 widget 是否为 **Image** 或其子类的实例），我们需要忽略它（返回 `false` 以将其从找到的 widgets 中排除）。如果是，我们需要检查它获取图像所用的 **ImageProvider** 是否实际上是 assets 中的 `icons/divide.png` 文件。

请注意，在单元测试和 widget 测试中访问 assets 功能仅从 Flutter 版本 *1.5* 开始可用。

### Flutter 集成测试

集成测试比 widget 测试高一个级别：对于移动应用，它在模拟器中执行，旨在测试应用的大部分功能是否正常工作。计算器应用非常简单，与 widget 测试相比，集成测试带来的收益并不多，而前一章的 XKCD 漫画应用则需要集成测试。

在我们对其运行集成测试之前，我们需要弄清楚如何在不依赖网络连接 XKCD 服务器可用以及无法访问文件系统（如单元测试和 widget 测试中的情况）的前提下，也运行其单元测试和 widget 测试。

### 测试 XKCD 应用：使用模拟对象

测试 XKCD 应用带来了一项挑战：该应用依赖于外部变量：需要存在互联网连接，并且应用需要能够连接到 XKCD 服务器（服务器需要处于启动状态）才能工作。问题在于，我们可能只需要测试应用的一小部分（尤其是在单元测试期间），而不需要与其他任何东西交互，而且由于无论如何我们都无法访问文件，因此我们需要模拟文件。为此，开发人员通常求助于构建*模拟对象*，这些对象模拟外部接口提供的功能，而不需要该接口。

构建模拟对象不是特定于语言或框架的，但 Google 开发了一个 Flutter 包，允许开发人员简化模拟对象的创建：Mockito 库。

#### MOCKITO 库

Google 开发的 *mockito* 包（在 Pub 上[^44]）允许你使用 `when` 函数来编写重现你所选择行为的类。

#### 编写模拟对象

举个例子，让我们编写一个用于 XKCD 应用中进行请求的 `http` 常量的替代方案，以便我们可以测试 `getLatestNumber` 和 `fetchComic` 方法。它是 `Client` 类的对象，所以我们编写一个空类，该类扩展（*extends*）Mock 并实现（*Writing a Class That implements an Interface*）Client 的成员：

```
```dart
import 'package:http/http.dart' as http;

class MockHTTPClient extends Mock implements http.Client {}
```
```

让我们定义两个我们可以返回的漫画：

```
```dart
// testing/xkcd_app/test/unit_test.dart
```
```

```
const comics = [
"""
{
"month": "",
"num": 1,
"link": "",
"year": "",
"news": "",
"safe_title": "The First Comic",
"transcript": "",
"alt": "first comic alt-text",
"img": "https://example.com/1.png",
"title": "The First Comic",
"day": ""
}
""",
"""
{
"month": "",
"num": 2,
"link": "",
"year": "",
"news": "",
"safe_title": "The Second Comic",
"transcript": "",
"alt": "second comic alt-text",
"img": "https://example.com/2.png",
"title": "The Second Comic",
"day": ""
}
"""
];
```

此时我们需要考虑我们对 http.read 的调用。第一个调用是 http.read(‘https://xkcd.com/info.0.json’)，它应该返回一个包含 JSON 格式漫画的 String，因此我们将返回第二个漫画：

```
when(mockHttp.read(‘https://xkcd.com/info.0.json’)).thenReturn(
comics[1]
);
```

同样的逻辑也需要应用于 /$num/info.0.json，因此我们还需要以下代码：

```
when(mockHttp.read(‘https://xkcd.com/2/info.0.json’)).thenReturn(
comics[1]
);
```

我们需要准备好返回编号为 1 的漫画，因为它将在某个时刻被获取，我们需要为此做好准备：

```
when(mockHttp.read(‘https://xkcd.com/1/info.0.json’)).thenReturn(
comics[0]
);
```

```
```dart
var file = File('${dir.path}/latestComicNumber.txt');
int n = 1;

try {
  n = json.decode(
    await http.read('https://xkcd.com/info.0.json')
  )["num"];
  file.exists().then(
    (exists) {
      if(!exists) file.createSync();
      file.writeAsString('$n');
    }
  );
}
catch(e) {
  if(file.existsSync() &&
     file.readAsStringSync() != ""
  )
    n = int.parse(file.readAsStringSync());
}
return n;
```
```

更新后的代码应该如下所示：

```
```dart
// testing/xkcd_app/lib/main.dart
Future<int> getLatestComicNumber
({http.Client httpClient, File latestComicNFile}) async
{
  if(httpClient == null) {
    httpClient = http.Client();
  }

  if(latestComicNFile == null) {
    final dir = await getTemporaryDirectory();
    latestComicNFile = File('${dir.path}/latestComicNumber.txt');
  }

  int n = 1;

  try {
    n = json.decode(
      await httpClient.read('https://xkcd.com/info.0.json')
    )["num"];
    latestComicNFile.exists().then(
      (exists) {
        if(!exists) latestComicNFile.createSync();
        latestComicNFile.writeAsString('$n');
      }
    );
  }
  catch(e) {
    if(latestComicNFile.existsSync() &&
       latestComicNFile.readAsStringSync() != ""
    )
      n = int.parse(latestComicNFile.readAsStringSync());
  }
  return n;
}
```
```



### 人生是一段美丽的旅程

敞开心扉拥抱每一刻，因为每一步都会带来新的奇迹。——佚名

当没有提供额外的命名参数时（它们会等于 `null`，从而触发 `if` 子句中的回退逻辑），此方法的工作方式与旧方法完全相同，这正是一般应用使用中的情况。不过，如果提供了这些参数，就会用它们来代替真实的文件和 HTTP 客户端。

显然，`fetchComic` 也需要修改。我们先从 `SelectionPage` 的那个方法开始，它在原应用中的代码如下：

#### networking/xkcd_app/lib/main.dart

```dart
Future<Map<String, dynamic>> _fetchComic(String n) async {
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
```

为了便于测试，需要改成下面这样：如果给定了客户端和文件参数就使用它们，否则（在正常应用使用中）回退到默认实现，就像我们对 `getLatestComicNumber` 所做的那样：

#### testing/xkcd_app/lib/main.dart

```dart
Future<Map<String, dynamic>> fetchComic
(
  String n,
  {
    http.Client httpClient,
    File comicFile,
    File imageFile,
    String imagePath
  }
) async
{
  int comicNumber = latestComic-n;
  Directory dir;
  if(httpClient == null) {
    httpClient = http.Client();
  }
  if(comicFile == null) {
    dir = await getTemporaryDirectory();
    comicFile = File("${dir.path}/${comicNumber}.json");
  }
  if(imageFile == null) {
    if(dir == null) dir = await getTemporaryDirectory();
    imagePath = '${dir.path}/${comicNumber}.png';
    imageFile = File(imagePath);
  }

  if(
    await comicFile.exists() &&
    comicFile.readAsStringSync() != ""
  )
    return json.decode(comicFile.readAsStringSync());
  else {
    comicFile.createSync();
    final comic = json.decode(
      await httpClient.read(
        'https://xkcd.com/${comicNumber}/info.0.json'
      )
    );

    imageFile.writeAsBytesSync(
      await httpClient.readBytes(comic["img"])
    );
    comic["img"] = imagePath;
    comicFile.writeAsString(json.encode(comic));

    return comic;
  }
}
```

通过把允许我们提供 mock 对象的额外参数设为可选，我们就可以在应用内部调用这些方法，而不必强制传入额外参数；同时我们让函数尽可能贴近原始版本，又允许使用 mock 对象。

### 编写测试

我们已经完成了必要的修改，现在需要为 XKCD 应用编写测试。

#### XKCD 应用单元测试

我们需要测试应用能否正确获取最新漫画编号（在本例中为 2）。我们还需要测试两个 `_fetchComic` 方法是否按预期运行。

首先，添加我们之前定义的 mock 类，并导入所有需要的内容。

在 `main` 之外，我们最后需要的是一个漫画数据字符串列表，而所有对 `when` 的调用都放在 `main` 函数定义内。

#### 测试 getLatestComicNumber

我们先从为 `getLatestComicNumber` 编写测试开始：为此，我们需要为最新漫画编号文件和 HTTP 客户端定义 mock 对象。

我们需要同时定义异步和同步调用的行为，因为我们需要测试也能适用于不同的实现：

#### testing/xkcd_app/test/unit_test.dart

```dart
test("get latest comic number test", () async {
  var latestComicNumberFile = MyFile();
  var latestComicNumberExists = false;
  String latestComicNumberString;
  var mockHttp = MockHTTPClient();

  when(mockHttp.read('https://xkcd.com/info.0.json')).thenAnswer(
    (_) => Future.value(comics[1])
  );

  when(latestComicNumberFile.createSync()).thenAnswer(
    (_) {
      latestComicNumberExists = true;
    }
  );

  when(latestComicNumberFile.create()).thenAnswer(
    (_) {
      latestComicNumberExists = true;
      return Future.value(latestComicNumberFile);
    }
  );

  when(latestComicNumberFile.writeAsStringSync("2")).thenAnswer(
    (_) {
      latestComicNumberExists = true;
      latestComicNumberString = "2";
    }
  );

  when(latestComicNumberFile.writeAsString("2")).thenAnswer(
    (_) {
      latestComicNumberExists = true;
      latestComicNumberString = "2";
      return Future.value(latestComicNumberFile);
    }
  }
);
⑧ when(latestComicNumberFile.existsSync()).thenReturn(
    latestComicNumberExists
);
⑨ when(latestComicNumberFile.exists()).thenAnswer(
    (_) =>
    Future.value(latestComicNumberExists)
);
⑩ when(latestComicNumberFile.readAsStringSync()).thenAnswer(
    (_) {
      // if we try to read from a file that doesn't exist we've made a mistake
      assert(latestComicNumberExists, true);
      return "2";
    }
);
⑪ when(latestComicNumberFile.readAsString()).thenAnswer(
    (_) {
      assert(latestComicNumberExists, true);
      return Future.value(latestComicNumberString);
    }
);
⑫ expect(
    await getLatestComicNumber(
      httpClient: mockHttp,
      latestComicNFile: latestComicNumberFile,
    ),
    2
  );
});
```

- ① 这里我们声明了 mock 对象以及它们操作的一些数据：一个使用 `bool` 来跟踪是否已创建的 mock `File`，以及一个 `String` 来模拟字符串存储。我们还声明了一个 mock HTTP 客户端。
- ② 这是我们第一个 `when` 调用，也是针对 mock HTTP 客户端的唯一一个：当我们尝试访问最新漫画时，它会返回我们伪造的最新漫画数据。
- ③ 现在我们需要定义 `latestComicNumberFile` 的行为：我们将从创建文件的同步方法开始，它会把 `latestComicNumberExists` 布尔值改为 `true`。当我们要从文件读取或运行 `exists` 或 `existsSync` 方法时，这一点很重要。

在圆括号中我们使用了 `_`，它用于表示未使用的参数（也可以用在 `catch` 块中）。如果我们选择使用该参数并给它命名，那它就会是一个 `Invocation`。

`Invocation` 有几个我们感兴趣的属性：

- `namedArguments`，一个由调用方提供的命名参数组成的 `Map`；
- `positionalArguments`，一个由调用方提供的位置参数组成的 `List`。

我们在异步创建文件的方法上也做了同样的事情，异步方法应当返回文件（因为是异步方法，所以返回的是 `Future`）。

在某个时刻，`getLatestComicNumber` 应尝试把数字 2 写入文件。我们会通过将它存储到 `latestComicNumberString` 中来模拟写入。同时我们也会模拟创建文件，就像真实的写文件方法所做的那样。

这和前一个类似，但是异步的。我们返回文件的原因和 `create` 中返回文件的原因相同。

`getLatestComicNumber` 会在某个时刻检查文件是否存在，因此我们也需要实现 `exists` 方法。

让 mock 对象实现被测方法中可能用到的方法（即使没有用到），可以使 mock 对象无需修改就能用于更多测试，并且如果我们在后续改变被测方法中使用的函数，也不必修改测试。在本例中，即使我们只使用其中一个，我们也会同时实现异步和同步方法。

这模拟了从文件读取。我们有一个断言，如果尝试读取尚未创建的文件（无论是通过运行 `create` 还是通过向文件写入内容），就会中断执行。

- 前一个方法的异步版本。

- 最后，我们来到了 `expect`，传入 mock HTTP 客户端和文件，并期望它返回 2。

#### 测试获取最新漫画

`fetchComic` 方法有一个重要特性：它实际上并不是以 `fetchComic` 命名的，而是叫做 `_fetchComic`，这使它成为私有方法：只能在类自身内部访问。好的实践是，所有不应从类外部访问的类成员都应定义为私有成员，但我们需要直接调用它来执行单元测试。这意味着我们需要把它的定义从：

```dart
Future<Map<String, dynamic>> _fetchComic
(
  String n,
  {
    http.Client httpClient,
    File comicFile,
    File imageFile,
    String imagePath
  }
) async
{
```

改为：

#### testing/xkcd_app/lib/main.dart

```dart
Future<Map<String, dynamic>> fetchComic
(
  String n,
  {
    http.Client httpClient,
    File comicFile,
    File imageFile,
    String imagePath
  }
) async
{
```

这也意味着我们需要把所有对 `_fetchComic` 的调用都改为对 `fetchComic` 的调用。在本例中，只有一处调用，它在 `build` 方法中被设置为 `FutureBuilder` 的 `future` 参数。

考虑到我们还需要测试 `SelectionPage` 和 `HomeScreen` 的 `fetchComic`，也需要对它们进行同样的修改。


那些类。StarredPage 的 fetchComic 仅在 _retrieveSavedComics 内部调用，而 HomeScreen 的 fetchComic 则被设置为 build 方法中 FutureBuilder 的 future。

### 测试 fetchComic

以下是测试 SelectionPage 的 fetchComic 方法能否获取第 2 号漫画的代码：

testing/xkcd_app/test/unit_test.dart

```
test("SelectionPage fetchComic latest", () async {
  var latestComicFile = MyFile();
  var latestComicExists = false;
  String latestComicString;
  var latestImageFile = MyFile();
  Uint8List latestImageData;
  var latestImageExists = false;
  var mockHttp = MockHTTPClient();

  when(mockHttp.read('https://xkcd.com/2/info.0.json')).thenAnswer(
    (_) => Future.value(comics[1])
  );

  when(mockHttp.readBytes("https://example.com/2.png")).thenAnswer(
    (_) => Future.value(Uint8List.fromList([2]))
  );

  when(latestComicFile.createSync()).thenAnswer(
    (_) {
      latestComicExists = true;
    }
  );

  when(latestComicFile.create()).thenAnswer(
    (_) {
      latestComicExists = true;
      return Future.value(latestComicFile);
    }
  );

  when(latestComicFile.writeAsStringSync(comics[1])).thenAnswer(
    (_) {
      latestComicExists = true;
      latestComicString = comics[1];
    }
  );

  when(latestComicFile.writeAsString(comics[1])).thenAnswer(
    (_) {
      latestComicExists = true;
      latestComicString = comics[1];
      return Future.value(latestComicFile);
    }
  );

  when(latestComicFile.existsSync()).thenReturn(
    latestComicExists
  );

when(latestComicFile.exists()).thenAnswer(
    (_) =>
        Future.value(latestComicExists)
);

when(latestComicFile.readAsStringSync()).thenAnswer(
    (_) {
        /*
         * if we try to read from a file that doesn't
         * exist it's because we've made a mistake
         */
        assert(latestComicExists, true);
        return latestComicString;
    }
);

when(latestImageFile.readAsBytes()).thenAnswer(
    (_) {
        assert(latestImageExists, true);
        return Future.value(latestImageData);
    }
);

when(
    latestImageFile.writeAsBytesSync(
        Uint8List.fromList([1])
    )
).thenAnswer(
    (_) {
        latestImageExists = true;
        latestImageData = Uint8List.fromList([1]);
    }
);

when(latestComicFile.writeAsString(comics[1])).thenAnswer(
    (_) {
        latestImageExists = true;
        latestImageData = Uint8List.fromList([1]);
        return Future.value(latestImageFile);
    }
);

var selPage = SelectionPage();

expect(
    await selPage.fetchComic(
        "2",
        httpClient: mockHttp,
        comicFile: File("latestComicFole"),
        imageFile: File("latestComicFile"),
        imagePath: "https://example.com/2.png"
    ),
    json.decode(comics[1])
);
```

这并不新鲜：我们使用 Mockito 为 fetchComic 方法在获取漫画时可能调用的所有函数定义预期行为：读写图像和漫画文件，以及执行 HTTP 请求。

为其他 `fetchComic` 方法定义新测试只需要修改 `HomeScreen`：它需要一个表示漫画编号的整数，该整数从 0 开始，最多比最新漫画编号小 1。

### 将测试分组

可以使用 `group` 方法将多个测试分组在一起，其用法与 `test` 完全相同。

```
group("group name", () {
  test("first test in group", () {
    // First test in group
  });
  test("second test in group", () {
    // Second test in group
  });
  // any other tests you want to add
});
```

例如，我们可以将所有涉及 `fetchComic` 方法的测试分组，或者，如果你决定以不同方式组织测试，也可以将所有获取特定漫画的测试分组。在分组内部，我们可以定义多个测试所需的变量和 mock 对象，这样组内所有测试都可以访问它们，但组外无法访问。不过需要记住，分组的回调闭包不能是 async 的。

> **更多单元测试：收藏漫画**
>
> 在 `StarredPage` 中测试漫画收藏并不会带来特别具有挑战性的情况，唯一需要注意的是 `retrieveSavedComics` 方法会调用 `fetchComic`，这意味着需要先将 mock 对象传递给 `retrieveSavedComics`，再由它传递给 `fetchComic`。
>
> 测试的整体结构应如下：应该在同一个文件上调用 `ComicPage` 中的 `addToStarred` 方法，随后 `retrieveStarredComics` 将使用该文件获取收藏漫画列表。此测试与 `fetchComic` 测试的主要区别在于，它需要两个模拟文本文件：其中一个需要包含漫画文件，而另一个将存储已保存漫画编号的列表，因此它需要预测将要被要求存储的数据，就像我们之前为漫画和最新编号文件所做的那样。
>
> 作为一个正在尝试理解 Flutter 的人编写这些测试，也会帮助你更详细地理解我们应用的内部工作原理，并检验你对其的理解。如果你无法为它编写测试，那么你可能并不理解它是如何工作的，这意味着你需要先尝试理解应用的工作方式。

### 浅谈更复杂应用的 Widget 测试

我们已经在关于测试计算器的章节中介绍过 Widget 测试，因此不会为 XKCD 应用提供 Widget 测试示例，那样会显得重复。

对 XKCD 应用进行 Widget 测试涉及测试 UI 是否真正工作，因此我们不一定需要设计能够完美模拟网络或文件系统行为的 mock 对象：我们只需要知道所需的 UI 存在，并且正在与我们的应用主干正确交互。

Widget 测试依赖于 mock 对象才能工作，因此在 Widget 测试中，应该做的只是将测试范围缩小到测试 Widget 是否调用了它们应该调用的函数：除此之外任何更多操作只会让测试更复杂，并让集成测试部分变得多余。

验证方法是否已被调用：Mockito 的 verify

Mockito 包含一个用于验证方法是否已被调用的函数，这使得将测试范围缩小到仅验证方法是否被调用成为可能，而不必检查某个方法是否返回某个给定值。例如，如果你希望在 `myObj.runMethod()` 未被调用时测试失败，你可以添加：

```
verify(myObj.runMethod());
```

如果将 `verify` 替换为 `verifyNever`，效果则相反。

你还可以指定方法应被调用的次数：

```
verify(myObj.runMethod()).called(5);
```

要求该方法已被调用 5 次，并且：

```
verify(myObj.runMethod()).called(greaterThan(4));
```

要求它已被调用*至少* 5 次。

遗憾的是，Flutter 的集成测试基础设施（在撰写本文时）并不支持这一点，这意味着我们必须创建一个 `TextEditingController` 和一个单独的 `FlatButton` 来提交 `TextField` 的内容。

### 定义 TextEditingController

`TextEditingController` 是一个类，用于存储插入到 `TextField` 或类似 Widget 中的数据，并使我们能够从赋给 `TextField` 本身的 `onSubmitted` 回调之外访问这些数据，从而允许我们创建一个打开用户所请求漫画的按钮，并在执行集成测试时为我们提供可点击的对象。

让我们将 `TextEditingController` 声明为 `SelectionPage` 的私有成员变量：

```
final TextEditingController _controller = TextEditingController();
```

### 使用 TextEditingController

让我们将它作为 `controller` 参数传递给 `TextField`：

```
TextField(
  controller: _controller,
  decoration: InputDecoration(
    labelText: "Insert comic #",
  ),
)
```

### 添加 FlatButton

我们现在需要将 `Scaffold` 的 `body` 中的 `Center` Widget 转换为 `Column`，这还需要将 `child` 参数改为 `children`，而 `children` 需要是一个 `List` 字面量（我们需要将 `TextField` 定义放在方括号中，并在其中添加 `FlatButton`）。

我们要添加的 `FlatButton` 需要一个在集成测试中使用的 `key`，并且它需要执行 `TextField` 的 `onSubmit` 所执行的相同操作，但通过获取


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

### 使用 flutter_driver 测试选择页面

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

#### 创建 selectionpage.dart 文件

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

#### 创建 selectionpage_test.dart 文件

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

### 更多测试

在我们的集成测试示例中，我们只测试了 SelectionPage。这是因为它允许我们组合许多有趣的 flutter_driver 特性，而且不需要重复多次相同的指令，这对于一个旨在帮助你学习新事物的示例来说不会太有趣。

我们没有对计算器应用进行集成测试，但它可以用来测试使用键盘执行计算，同时测试计算和 UI，这不过是我们已经做过的测试的组合：它在真实应用中有用，但你已经知道如何去做。这就是集成测试的用途，就像我们为 XKCD 应用编写的测试一样：它同时测试 UI 和逻辑。

测试代码库至关重要，应该尽可能多地进行。在编写自己的应用时，重要的是你为应用的每个方面设计和编写全部三种测试：我们的 XKCD 应用需要针对 HomeScreen、ComicPage 进行测试，也许还需要测试在需要时是否显示 ErrorPage。此外，更多针对基本 UI 元素的单元测试和组件测试也很有用（例如，测试 HomeScreen 上的 AppBar 控件）。

这些测试用例的代码示例与之前的很相似，因此我没有在此复现它们。有一种方法可以让测试更容易，但不太可预测或可控，因为它依赖于本地机器的文件系统，如果测试在服务器上运行且不允许代码写入文件系统，则可能无法访问。

另外，一个即使在集成测试中也绝对需要 mock 对象的重要特性是身份验证：需要身份验证才能工作的应用会使测试无法进行，除非测试利用 mock 来绕过身份验证的问题。

### Mockito 与自建 Mock 对象对比

Mockito 并不总是让创建 mock 对象更容易：相反，有时你可能会发现直接编写一个实现所需方法的类更容易。你可以通过将这些类定义为子类（*extends*）或定义为实现（*编写一个实现接口的类*）你想要 mock 的接口的类来使它们工作。





### 抛出和捕获异常

我们在前面的章节中已经使用过异常，但本节将更详细地介绍 Flutter 中的异常实现。

异常在代码片段中被用作一种信号机制，表明某些地方出现了错误。当前函数（或*匿名函数和闭包*）的执行会被中断，而调用该函数的一方应该通过捕获异常来处理这一问题。

#### 抛出异常

在 Dart 中，使用 `throw` 关键字来抛出异常（如果你在 Python 中，你会称这为引发异常；如果你还没接触过 Python 之外的世界，可以了解一下）。你可以通过创建一个实现 `Exception` 类的类来创建自己的异常（参见*编写实现接口的类*）：

```dart
class MyOwnPersonalException implements Exception {
  MyOwnPersonalException(this.myOwnPersonalMember);

  String myOwnPersonalMember;
}

double myPersonalDivision(double a, double b) {
  if(b == 0) {
    throw MyOwnPersonalException("Failed to divide a number by 0.");
  }
  return a/b;
}
```

#### 捕获异常

每当我们调用一个可能在某些未检查的情况下失败或可能失败的函数或操作时，我们都应该捕获异常：

```dart
void responsibleFunction() {
  int a, b;
  // ...
  b = 0;
  try {
    int d = myPersonalDivision(a, b);
    // ...
  }
  catch(exception) {
    // 处理异常
  }
  // ...
}
```

你可以将 `catch` 括号中的 `exception` 替换为任何你想要的名称（通常只是 `e`）。它包含了被抛出异常的名称。你可以使用 `print(exception)` 将其打印到调试控制台中。此外，你还可以传入错误的源栈跟踪，也可以将其打印出来。

例如，以下内容：

```dart
try {
  int.parse("Flutter");
} catch(e, stacktrace) {
  print(e);
  print(stacktrace);
}
```

将触发一个 `FormatException` 并在调试控制台中打印以下内容：

```
FormatException: Invalid radix-10 number (at character 1)
Flutter
^
#0 int._throwFormatException
(dart:core/runtime/lib/integers_patch.dart:131:5)
#1 int._parseRadix (dart:core/runtime/lib/integers_patch.dart:142:16)
#2 int._parse (dart:core/runtime/lib/integers_patch.dart:100:12)
#3 int.parse (dart:core/runtime/lib/integers_patch.dart:63:12)
#4 main (file:///home/carmine/flutter/example_app/lib/main.dart:21:9)
```

##### 使用 on SomeException catch()

如果被调用的函数抛出了另一种类型的异常，该异常没有 `myOwnPersonalMember`，或者你需要以不同方式处理不同的异常怎么办？这正是 `on` 关键字的用途。

例如，假设我们有一个函数，它接收两个字符串形式的数字，解析它们并对它们执行整数除法。这意味着在执行期间可能会抛出两种不同类型的异常：如果无法解析数字，则抛出 `FormatException`；如果我们尝试除以零，则抛出 `IntegerDivisionByZeroException`。

Dart 默认执行浮点数除法并返回双精度浮点数。要执行整数除法，我们需要使用 `~/` 运算符，而不是仅仅使用 `/`。这意味着函数定义将是：

```dart
int jsonDivide(String a, String b) =>
  int.parse(a) ~/ int.parse(b);
```

看起来就是这样，难怪像这样的函数是一个很好的例子：它非常简短，但可能会抛出两种不同类型的异常：例如，将其调用为 `jsonDivide("5", "0")` 会返回一个 `IntegerDivisionByZeroException`，而将其调用为 `jsonDivide("Flutter", "10")` 会返回一个 `FormatException`。假设我们需要告诉用户纠正他们输入的错误数据，因此我们需要以不同的方式处理每个异常，这在 Dart 中并不难实现：

```dart
try {
  jsonDivide(firstString, secondString);
}
on IntegerDivisionByZeroException {
  // 告诉用户不要除以 0
}
on FormatException {
  // 告诉用户输入有效的数字
}
```

这样，两个 `on` 块中的每一个都只有在抛出相应异常时才会执行。

`on` 块的另一种语法是 `on ExceptionName catch(e) {}`，这将把异常传递给该块，就像在普通的 `catch` 块中发生的那样。

可选地，你可以在 `on` 块之后添加一个 `catch` 块，以处理未被 `on` 块处理的任何异常。

#### FINALLY

在使用普通的 try-catch 构造且在此过程中不抛出新异常时，执行将通过执行 catch 块末尾之后的指令继续。但如果你只使用 `on` 块而不是通用的 `catch` 块，或者你在 `catch` 块内抛出新的异常，执行将被中断，那些指令将不会被执行。相反，如果你想执行某些指令，无论 `try` 块、`catch` 和 `on` 块中发生什么，你都会使用 `finally` 块：

```dart
try {
    // 可能会失败的代码
}
on SomeException {
    // 处理 SomeException
}
finally {
    // 肯定会执行的代码
}
```

#### 断言语句

在测试用例的使用和异常使用之间，你会找到断言语句。这是开发人员定义在应用程序执行期间必须满足的条件，或有效地测试特定条件结果的一种方式。

断言语句可以添加在应用程序代码的任何位置，并在条件不返回 true 时通过抛出 `AssertionError` 异常来终止应用程序的执行：

```dart
int a = 3;
assert(a == 3);
```

这两行代码不会影响应用程序的执行，类似的语法经常在演示编程语言特性的代码示例的文档中使用，包括谷歌官方的 Dart 语言指南 [45]。

另一方面，拥有如下代码：

```dart
int a = (6+9/3)/3;
assert(a != 3);
```

将在*调试过程中*通过抛出异常终止应用程序的执行。断言语句对生产代码没有影响，仅在调试代码时被视为有效。

可以通过将字符串作为第二个命名参数传递给 `assert` 来包含一个描述断言的字符串：

```dart
assert(condition, "The condition wasn't met");
```

#### 接下来的内容

在下一章中，我们将讨论 Firebase，以及它如何让构建聊天应用程序变得非常简单。

#### 脚注

- [43] https://docs.flutter.io/flutter/flutter_test/CommonFinders-class.html
- [44] https://pub.dartlang.org/packages/mockito
- [45] https://www.dartlang.org/guides/language/language-tour
