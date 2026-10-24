

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

## 进行自定义修改

如果你在计算器应用中实现了括号功能，也应该考虑为其编写一个单元测试。

编写单元测试非常简单：实际上，你只需要在想要添加运算符时拆分 `calc.add` 调用即可。不过，建议在至少部分测试中逐个数字添加数字，因为这是实际计算器应用的运行方式，而且可能存在仅在这种操作方式下才会显现的问题（也许一次添加多位数字会破坏在真实计算器中能正常工作但在我们实现中会出错的逻辑，但就我们的实现而言情况并非如此）。

在此阶段，每次尝试构建你的应用时，这些测试都会运行。你也可以在命令行使用 `flutter test` 来运行测试。然而，使用 Visual Studio Code 运行测试可能是最好的方式，它为每个测试显示一个运行按钮，允许你运行每个测试并在直观的 UI 中查看结果。Android Studio 也在行号旁边显示一个向右的箭头按钮来运行测试，但我发现其 UI 略显缺乏直观性和一致性。不过，你的感受可能不同，尤其是如果你更喜欢 Android Studio/IntelliJ 的 UI 而不是 Visual Studio 的 UI；请选择你更习惯使用的工具。

## FLUTTER 小部件和集成测试

单元测试足够简单直接，但小部件测试和集成测试需要更多关注，因为它们依赖于 UI，而 UI 并不像计算处理代码那样枯燥和简单。

我们应用中的每个低级逻辑片段可能是正确的，但 UI 或某些已测试代码片段的组合中可能存在错误。这就是小部件测试和集成测试存在的原因。它们用于测试单个小部件或应用的更大部分是否正常工作。

## Flutter 小部件测试

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

## 所有的 Finders

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

