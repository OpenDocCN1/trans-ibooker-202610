
```dart
var list = numbers.map((el) => "#$el").toList();
```

## 下划线

你可能在许多代码示例中看到过，我们在一些变量或类名前面加了下划线（_）。这是为了使它们成为*库私有*。

例如，当我们编写：

```
```dart
class MyWidget extends StatefulWidget {
  // (...)
}

class _MyWidgetState extends State<MyWidget> {
  // (...)
}
```
```

我们使 MyWidgetState 类成为*库私有*，这意味着它只能从当前包内访问：如果你正在开发一个库，并且希望让该库的用户无法访问某个类或顶层函数，你可以这样使用下划线。

对类成员使用下划线是为了使其无法从类定义之外访问，如下例所示：

```
```dart
class CounterClass {
  int _counter = 0;

  void _setCounterTo(int n) {
    _counter = n;
  }

  int getCounter() => _counter;

  void increaseCounter() => _setCounterTo(_counter+1);
}
```
```

这显然比实际需要的复杂得多（_setCounterTo 增加了不必要的复杂性），这是为了展示如何选择不同类型的方法将其设置为私有：如果我们创建一个计数器类，我们希望能够获取计数器的当前值，但我们不需要将其更改为除递增 1 之外的任何值，因此我们只提供访问一个恰好做到这一点的方法，以及另一个返回计数器值的方法。

## 其他可迭代数据结构：Map、Set 等

Map 是一种键/值集合，类似于 JSON 对象，通常用作 Dart 中处理 JSON 对象的方式，作为为应用使用的每种对象定义类的更简单替代方案。Map 字面量用花括号括起来，看起来像这样：

```
var user = {
  "userId": 1,
  "firstName": "Carmine",
  "lastName": "Zaccagnino"
};
```

你可以使用与访问和修改 List 元素相同的 [] 和 [] 来访问和添加 Map 元素：

```
var name = "${user['firstName']} ${user['lastName']}";
user["password"] = "my!!Very!!own!!Very!!safe]passWordstr";
```

Set 是不同值的线性集合：

```
var G8 = {
  "United States",
  "Japan",
  "Germany",
  "United Kingdom",
  "France",
  "Italy",
  "Canada",
  "Russia"
};
```

你可以使用 remove 按值从 Set 中移除一项，并使用 add 添加一项：

```
var G7 = G8
  ..remove("Russia")
  ..add("European Union");
```

### Set 字面量

Set 字面量

从 Dart 2.2 版本开始支持使用花括号的 Set 字面量，在更早的版本中，最类似的方法是使用 Set.from 方法，它接受一个 List：

```
var countriesByGDP = Set.from(["USA", "PRC", /*...*/]);
```

还有更多可迭代对象。计算机科学中非常常见的一种是链表，可以使用 LinkedList 实现，但还有 Queue 类，以及基于无序哈希表的 Map 和 Set 替代品，称为 HashMap 和 HashSet。

## 数字：NUM 类型及其子类

num 类是一个通用的数字类，其子类是用于整数的 int 和用于浮点数的 double。Dart 中没有 float；浮点数都是 64 位双精度。

与英国和美国使用的数学符号一致，小数点分隔符是点（.）。你可以使用 e 指定 10 的幂：

```
var a = 5.2e2;

assert(a == 520);
```

double 数字可以使用 round 进行四舍五入（当小数部分从 .5 开始时会远离 0 舍入），使用 floor 向下取整，或使用 ceil 向上取整：

```
var a = 5.2;

assert(a.round() == 5);
assert(a.floor() == 5);
assert(a.ceil() == 6);
```

```
var b = 5.5;

assert(a.round() == 6);
```

十六进制整数通过在其前面加上 `0x` 获得：

```
assert(0x7E3 == 2019);
```

## 字节列表及其他：dart:typed_data 库

在读取二进制数据时，如果每个 64 位整数所做的事情明显低于最优效率，那么使用 List<64 位 int> 是非常低效的。这就是为什么 dart:typed_data 包含 Uint8List 类，它的工作方式与常规 List 完全相同，但使用 8 位整数。

## 类型转换

在 Dart 中，类型转换可以通过以下两种方式之一完成。

你可以使用 `as` 表达式将值转换为其子类型之一：

```
num a = 5;

var a_int = a as int;

assert(a_int is int);
```

或者使用 cast 将 List 的所有值转换为指定类型：

```
List<num> l = [1, 2, 3];

var l_int = l.cast<int>();

assert(l_int[0] is int);
```

## 函数

Dart 支持顶层函数，这意味着你可以在任何类定义之外定义函数，这些函数可以从代码中的任何位置调用。

Dart 函数的定义方式如下：

```
returnType functionName(parameters) {
    blockOfInstructions;
    return returnValue;
}
```

其中：
- returnType 是函数将返回的数据类型，请参阅*原生 Java/Apple 与 Dart 数据类型之间的转换*一节。
- 参数将在下一段中解释，它们是函数用来完成其任务的数据。参数是变量，其值通过函数调用参数设置。如果你不熟悉函数和函数调用，下面的示例旨在提供几个利用其他函数的真实、可工作的示例。
- 花括号内是添加指令块（一行或多行）的地方，如果函数有任何非 void 的返回类型，则包含一条 return 指令，用于向调用代码返回一个值。

例如，以下函数非常简单：

```
double square(double n) {
    return n*n;
}
```

但它可以在一个函数内部用于计算引力，该函数稍微复杂一些，有更多参数：

```
// We'll see how to set all of those arguments soon
double calculateGravitationalAttraction(double mass1, double mass2, double distance)
{
    const G = 6.67e-11; // universal gravitation constant
    /*
    * Calling the square function using distance as
    * the only argument (n will be equal to
    * the value in the distance variable)
    * and saving the return value
    * to the squaredDistance variable
    */
```

变为：

```
int square(int n) => n*n;
```

通常写成，尤其是当表达式较长时，如下所示：

```
int square(int n) =>
  n*n;
```

## 匿名函数和闭包

闭包是我们用来定义回调的方式。它们是定义为变量的函数，或者更具体地说，是 Function 类型或 Typedef 和 Callbacks 的变量。

## main

main 是当我们启动任何类型的 Dart 程序（包括 Flutter 应用）时被调用的函数。它的返回类型为 void，不接受任何参数。它可以是异步的。

## Dart 中的异步代码：`dart:async` 库

在 Dart 开发过程中的某个时刻，你可能需要在不影响代码执行的情况下调用一个方法，或者调用一个已声明为异步方法的库方法。

这时你就会意识到异步代码和 Dart 内置的 `dart:async` 库的必要性，它允许异步函数返回 `Future` 或 `Stream`。

## Future

`Future` 是与异步方法相关的最基本的对象类型——它是所有异步函数（包括 void 函数）的返回类型：

```
```dart
Future<void> doSomethingThatTakesLong() async {
  // do something
}
```
```

```
```dart
void callingSyncFunction() {
  doSomethingThatTakesLong();
  /* the code won't wait for the function to
   * finish its execution and it will carry on
   * in its regular order, executing
   * the next instruction
   */
}
```
```

如果你需要等待函数停止执行（也许是因为你需要返回值），你需要在 `await` 和 `.then` 之间做出选择。

## await

当你确实需要等待异步函数完成执行时（通常是因为你需要它的返回值），`await` 非常有用。它通过等待返回 future 的代码完成执行，将 `Future` 转换为函数返回的值：

```
```dart
Future<int> getValue() async {
  return 5;
}
```
```
