## Appendix 1

### Introduction to Dart

Dart is a new language, and you might not be aware of its syntax and some of its features. Here I’ll provide an overview of Dart so that you can have a better grasp on how it works.

Dart, in many ways, resembles C, C++, and Java, so developers with good knowledge of those languages shouldn’t have too many issues understanding the code in the book, but there are also differences that I’ll point out along the way.

This appendix is also a good starting point for people who don’t have much programming experience.

### Comments in Dart

Comments are sections of code that won’t be evaluated by the compiler and won’t be executed. Just like in C, Java, JavaScript, and similar languages, comments are created in two ways:

- Single-line comments are created by adding `//` before the comment.
- Multi-line comments are created by adding `/*` before the comment and `*/` after it.

For example:

```
// this is a comment
int a = 3; // this is also a comment
/* this is also
  a long, long
comment */
```

where `int a = 3;` is regular code (not a comment) and will be evaluated by the compiler and executed.

### Variables and Conditions

Variables can be declared in two ways in Dart:

- In the *traditional* C-like way, by prefixing the variable name with the data type:

  ```
  int a = 3;
  ```

- By prefixing the variable name with `var`:

  ```
  var a = 3;
  ```

A list of some of the most common data types is provided at the end of this appendix.

### Characters and Strings

In some languages there are `char` types for single characters and arrays of chars or separate types for strings. In Dart, the `String` type is the only character/string type, and it can contain one or more characters. For that reason, unlike in other languages, you can use single or double quotes to enclose what you assign to the string (called a *string literal*):

```
String greeting = "Hi!";
```

will work just as well as:

```
var greeting = 'Hi!';
```

Like in many other modern languages, two strings can be concatenated using the same `+` operator you use for addition between numbers:

```
var firstGreeting = "Hi" + "!";
assert(firstGreeting == 'Hi!');
```

What’s `assert`? It’s something you will get used to seeing reading this appendix: its effect in compiled code is that, in a testing/debugging situation (it does nothing on production code), it terminates the execution if the condition in its parentheses isn’t true, but it’s widely used in programming language tutorials and documentation to explain language features. Whenever you see one in this appendix, you should expect the condition inside its parentheses to always be true.

### The Backslash

Just like in many other languages, the backslash can be used to escape some characters or insert special characters.

For example, if you wanted to have quotes inside a string, you have two ways to do it:

- You use different kind of quotes to enclose the string (for example `'He said: "Hi!"'`).
- You escape the quotes (for example, `"He said: \"Hi!\""`).

For the special characters part, this will be familiar especially to C developers; the `\n` character can be used to go to a new line:

```
Text("First line\nSecond line")
```

will render *Second line* below *First line*.

Obviously, the backslash itself has to be escaped to be used as part of a string:

```
Text("\\ is the backslash character")
```

will display `\ is the backslash character`.

### String Interpolation

String interpolation allows you to insert one or more variables’ value into a string. Let’s first explore how things would be done *without* string interpolation.

For example, if you wanted to display the value of an `int` with a `Text`, you can’t just write:

```
int number;

// (...)

Text(number)
```

because `Text` expects a `String`. This means you need to change that to:

```
Text(number.toString())
```

What if you want to display a string and the number?
Then you would need to write something like this, using string concatenation:

```
"You tapped the button " + number.toString() + " times"
```

What string interpolation allows you to do is write this instead:

```
"You tapped the button $number times"
```

By prefixing a variable’s name with `$` you can insert its value into the string. If you need to access an element of a `List`, the member of a class, or write any kind of expression, you need to enclose the statement in curly braces. If you had a list like the following:

```
List numbers = [1, 2, 3];
```

you would use string interpolation like this:

```
"There are ${numbers.length} and second element is number ${numbers[1]}"
```

### Multi-Line Strings and Raw Strings

Like in many other languages, you can make multi-line strings by using triple quotes and you can make raw strings by prefixing the opening quotes with the letter “r”.

A multi-line string makes it so that you can turn this:

```
"First line\nSecond line";
```

or this:

```
"First line\n" +
```

### Lists

A `List` is what is commonly referred to as an array: an ordered collection of data, the elements of which are indexed with the values from `0` to `length-1`, where the `length` of a `List` is the number of elements it contains. A `List` can be initialized with a list literal, which is a comma-separated list of values enclosed in square brackets:

```
var list = [30, 20, 500];
assert(list[0] == 30);
assert(list[1] == 20);
assert(list[2] == 500);
```

### If and For in List Literals

List literals in Dart can do more than that, though—you can have `if` statements in them:

```
bool condition;
// ...
var list = [
    if(condition) "First"
    else "First, alternative",
    "Second"
];
assert(list[1] == "Second");
if(condition)
    assert(list[0] == "First");
else
    assert(list[0] == "First, alternative");
/*
 * As in other languages, these last two assert() calls could become one:
 * assert((condition && list[0] == "First") ||
 *        (!condition && list[0] == "First, alternative"))
 */
```

### Adding and Removing Items from Lists

An item can be added to the end of a list by using `List.add`:

```
var l = [1, 2, 3];
l.add(4);
assert(l[3] == 4);
```

An item can be removed in two main ways (there are others, feel free to check out all `List` methods at the official Dart language reference [55]). One of them is by passing the value you want to remove with `List.remove` (which will only remove the first occurrence):

```
var l = ["Test", "test", "Test"];

l.remove("Test");

assert(l[0] == "test");
assert(l[1] == "Test"); // The second occurrence won't be removed
```

The other is by passing the index at which you want to remove an element to `List.removeAt`:

```
var l = [5, 6, 7];

l.removeAt(1);

assert(l[1] == 7);
```

### Generating Lists

One of the ways to create lists is by using the `List.generate` constructor, which takes two arguments: the number of elements the list will have and a callback function (a function to be run by another function in certain conditions and with certain arguments) that generates each element based on its index.

For example, this is how you’d create a list from the concatenation of the strings contained in two lists:

```
var l1 = ["First", "Second", "Third"];
var l2 = ["1", "2", "3"];

var l = List.generate(3, (index) => l1[index] + l2[index]);
```

If this example wasn’t clear to you, check out the section about functions (Functions) and the one about classes (Classes).

### Iterating Over Lists: for, map, and forEach

We’ve found out how to generate lists, now what about how to iterate over the elements of a list? There are multiple ways to do that:

- The `for in` loop.
- The `.forEach` method.
- The `.map` method.

### The for in Loop

Present in most modern languages, it’s a special kind of loop: `while`, `do while`, and `for` that iterates over the elements of a list:

```
var listName = [
  "First",
  "Second",
  "Third",
  "Fourth"
];

for(var elName in listName) {
  print(elName);
}
```

### map and forEach

Particularly loved by some for their functional style, the `Iterable`'s `map` and `forEach` methods are alternatives to the `for in` loop.

`forEach` is the most similar to the `for` loop, as it is used to execute an operation on (or for) each of the elements of a list. The callback closure you use is of type `void`, so you can’t return anything. Here’s an example, showing how to print each element of a list:

```
listName.forEach((el) {
  print(el);
});
```

`map` is different: it is used to create an `Iterable` from calling a function on each element of the `List`, here’s an example:

```
var numbers = [
  1,
  2,
  3,
  4,
  5,
  6
];

var iter = numbers.map((el) => "#$el");
```

`iter` is an `Iterable`, not a proper `List`. If you want to access the elements directly just like you do with a list, you need to use the `toList` method:


```dart
var list = numbers.map((el) => "#$el").toList();
```

### 下划线

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

### 其他可迭代数据结构：Map、Set 等

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

#### Set 字面量

Set 字面量

从 Dart 2.2 版本开始支持使用花括号的 Set 字面量，在更早的版本中，最类似的方法是使用 Set.from 方法，它接受一个 List：

```
var countriesByGDP = Set.from(["USA", "PRC", /*...*/]);
```

还有更多可迭代对象。计算机科学中非常常见的一种是链表，可以使用 LinkedList 实现，但还有 Queue 类，以及基于无序哈希表的 Map 和 Set 替代品，称为 HashMap 和 HashSet。

### 数字：NUM 类型及其子类

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

### 字节列表及其他：dart:typed_data 库

在读取二进制数据时，如果每个 64 位整数所做的事情明显低于最优效率，那么使用 List<64 位 int> 是非常低效的。这就是为什么 dart:typed_data 包含 Uint8List 类，它的工作方式与常规 List 完全相同，但使用 8 位整数。

### 类型转换

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

### 函数

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

### 匿名函数和闭包

闭包是我们用来定义回调的方式。它们是定义为变量的函数，或者更具体地说，是 Function 类型或 Typedef 和 Callbacks 的变量。

### main

main 是当我们启动任何类型的 Dart 程序（包括 Flutter 应用）时被调用的函数。它的返回类型为 void，不接受任何参数。它可以是异步的。

### Dart 中的异步代码：`dart:async` 库

在 Dart 开发过程中的某个时刻，你可能需要在不影响代码执行的情况下调用一个方法，或者调用一个已声明为异步方法的库方法。

这时你就会意识到异步代码和 Dart 内置的 `dart:async` 库的必要性，它允许异步函数返回 `Future` 或 `Stream`。

### Future

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

### await

当你确实需要等待异步函数完成执行时（通常是因为你需要它的返回值），`await` 非常有用。它通过等待返回 future 的代码完成执行，将 `Future` 转换为函数返回的值：

```
```dart
Future<int> getValue() async {
  return 5;
}
```
```




### 创建流

创建 [Stream](#) 最基本的方式是从异步生成器函数 ([async*](#)) 中返回多个值，使用 [yield](#) 关键字。

#### 从生成器函数中产生值

生成 [Stream](#) 的函数看起来像这样：

```
Stream<String> myStream() async* {
  var returnValues = [
    "This is The First String You'll See",
    "After 2 Seconds You'll See This",
    "Then This"
  ];
  for (int i = 0; i < 3; i++) {
    await Future.delayed(Duration(
      seconds: 2
    ));
    yield returnValues[i];
  }
}
```

[Future.delayed](#) 允许你定义一个 [Future](#)，可用于暂停执行（通过等待其完成）指定时间。

[yield](#) 就像 [return](#)，但它不会将执行移出异步生成器函数：它将值产生给调用函数，然后在生成器函数内部恢复执行，使其以后可以再次产生另一个值。

#### 使用 StreamController

[StreamController](#) 是生成 [Stream](#) 的更高级方式：它允许你使用常规同步函数生成 [Stream](#)，并在更复杂的场景中使用，在这些场景中 [yield](#) 就不够用了。

一旦在函数中定义了 [StreamController](#) 变量，你就可以在函数内的任何位置使用 [StreamController.add](#)（例如在回调中），其效果类似于 [yield](#)。另一个非常重要的 [StreamController](#) 方法是 [StreamController.close](#)，它会导致监听函数停止监听 [Stream](#)。

StreamController 有两个构造函数：StreamController（默认）和 StreamController.broadcast。

StreamController 构造函数，用于仅应有一个监听器的流，接受四个回调作为参数：

- onListen，当监听器开始监听对象时触发，应触发值的产生开始，以避免因触发事件而无监听器监听，导致 Dart 缓冲它们，消耗内存用于可能永远不会被监听的事件。
- onPause，当监听器请求 Stream 暂时不产生任何内容时触发，使用 StreamSubscription.pause，原因与 onListen 相同。
- onResume，当监听器通过调用 StreamSubscription.resume 请求 Stream 在暂停后恢复产生值时触发，很明显此时该函数应导致流恢复触发事件。
- onCancel，当监听器通过调用 StreamSubscription.cancel 取消订阅时触发， supposed to make the Stream stop trying to yield values.

StreamController.broadcast 构造函数，用于广播流，可以被多个监听器监听。它仅接受 onListen 和 onCancel 作为参数。

### 使用流

我们在《获取电池和连接数据：使用流进行异步编程》中已经看到了使用 Streams 的基础。那个示例遵循以下结构：

```
class MyWidget extends StatefulWidget {
  //...
}

class MyState extends State<MyWidget> {
  //...
  StreamSubscription<Type> _streamSub;

  @override
  void initState() {
    //...
    _streamSub = stream.listen(
      (Type res) {
        // do something with the response, setState()
      }
    );
  }

  @override
Widget build(BuildContext context) {
  //...
}

@override
dispose() {
  //...
  _streamSub.cancel();
}
```

在这种情况下，流直接由库提供，我们使用 setState 在有新数据可用时更新一些变量并重建 widget。

如果每次流发生变化时被重建的 StatefulWidget 并不是仅显示该 Stream 数据的部分，那么所有这些都是低效的。

#### StreamBuilder

为此单独构建一个 StatefulWidget 并不是必要的，因为，就像 FutureBuilder 用于基于 Future 的结果构建 UI 元素一样，StreamBuilder 是 Flutter widget，它根据 Stream 返回的数据构建其内容。

StreamBuilder 的构造函数需要，就像 FutureBuilder 一样，一个数据类型和两个参数，但有一些不同。

StreamBuilder widget 的基本结构如下：

```
StreamBuilder<Type> {
  stream: stream,
  builder: (context, snapshot) {
    if(snapshot.hasData)
      // display snapshot.data
    else
      // display an error or some indication that the data is loading
  }
}
```

StreamBuilder 与 FutureBuilder 的一个区别在于 snapshot.connectionState 值。当在 FutureBuilder 中使用 Future 时，它只能假设以下值：

- ConnectionState.none 如果 future 参数设置为 null（根本未设置）。
- ConnectionState.waiting 如果 FutureBuilder 正在等待 future 返回值。
- ConnectionState.done 如果 future 已返回值，意味着 snapshot.hasData 为 true。

这就是为什么我们在使用 Futures 和 FutureBuilders 时只使用 snapshot.hasData 而不使用 snapshot.connectionState。当使用 Streams 时则不同——连接状态也可能是 ConnectionState.active，意味着 Stream 已经返回值，但它尚未终止，意味着它随时可能返回另一个值。

snapshot.hasError 和 snapshot.error 在两者中都存在，以防你需要处理错误。

#### await for 循环

Stream 可以通过另一种方式与之交互，而不是使用 listen 方法：await for 循环。

它就像 for-in 循环，但不是遍历列表中已知的值，而是等待流返回的新值，并且仅在流终止时结束：

```
await for(var value in stream) {
  // do something with the value
}
```

例如，我们可以使用以下循环在 myStream 函数产生它们时立即将它们打印到控制台：

```
await for(var res in myStream()) {
  print(res);
}
```

await for 不仅可以打印结果，还可以例如获取现有 Stream，改变数据或将其用作其他函数的参数，然后产生改变后的值，从而创建新的 Stream。

#### 流内的错误处理

在使用 StreamBuilder 时，builder 中获得的 snapshot 具有与 FutureBuilder 相同的 snapshot.hasError 和 snapshot.error，因此如果需要在 FutureBuilder 或 StreamBuilder 中处理错误，可以使用它们。

listen 方法可选地接受另外两个回调作为参数：

- onError，当生成 Stream 的异步函数抛出错误时运行。
- onDone，当 Stream 终止时运行。

### 条件构造和表达式

某些语言（如 Rust）将条件构造视为表达式。Dart 仅在列表字面量内部这样做；在其他地方你需要使用条件表达式，采用使用三元 `?:` 运算符的常规 C 风格语法。

#### IF-ELSE 构造

Dart 中的 if-else 构造与许多其他语言非常相似；它可以使用大括号表示：

```
if(condition) {
  // Instructions to execute when the condition is true
} else {
  // Instructions to execute when the condition is false
}
```

如果每个条件只有一个指令要执行，则无需大括号（在条件为真时执行 `true_instruction`，在条件为假时执行 `false_instruction`），通常不换行书写：

```
if(condition) true_instruction;
else          false_instruction;
```

#### else if

如果需要在 else 子句中检查一个条件，可以使用 `else if` 构造：

```
int a;
// (...)
if(a>10) {
  // (...)
} else if(a == 10) {
  // (...)
} else {
  // (...)
}
```

#### 条件表达式

如果需要测试一个条件以决定赋值的两个值、一个表达式或一个返回


statement 你可以使用条件表达式。它是一个包含 ?: 三元运算符的表达式，可以按以下方式使用：

```
var a = condition ? true_value : false_value;
```

其中，如果条件为真，则 a 的值为 true_value，如果条件为假，则 a 的值为 false_value。这对于替换如下简单函数非常有用：

```
num max(num a, num b) {
  if(b > a) return b;
  else return a;
}
```

使用更简单的单行代码：

```
num max(num a, num b) => b > a ? b : a;
```

### SWITCH

Switch 是 Dart 中可能看起来最古老的功能之一：它与 C 或 Java 的用法完全相同。它们用于在需要将变量与一定数量的常量值进行比较时使用，不推荐在 Flutter 应用中使用。其结构如下：

```
var a;
// (...)

switch(a) {
  case 1:
    // 如果 a 等于 1 则执行的指令
    break;
  case 2:
    // 如果 a 等于 2 则执行的指令
    break;
  default:
    // 如果 a 既不等于 1 也不等于 2 则执行的指令
}
```

如果省略 break 指令，则由变量值触发的 case 之后定义的情况也会被执行。这种效果（fall-through）允许多个值由单个 case 处理：

```
switch(a) {
  case 1: // 这是 supposed to fall through
  case 2:
```

```
// 值 1 和 2 的指令
break;
case 3:
// 值 3 的指令
break;
default:
// (...)
}
```

如果 case 中有指令，通过省略 break 获得的 fall-through 不被允许并且会生成错误。如果你的确想这样做，应该使用带标签的 continue 指令：

```
switch(a) {
  case 1:
    // 1 的额外指令
    continue caseTwo;
  caseTwo:
  case 2:
    // 1 和 2 的指令
    break;
  default:
    // (...)
}
```

### Loops: while, do while, and for

Dart 支持最常见的循环结构，for 循环支持 for in 记法，并且从 Dart 2.3 开始可以在列表/集合字面量内部使用：

```
var list1 = [
  for(int i = 0; i < 100; i++)
    i,
];
```

```
var list2 = [
  for(var element in list1)
    "#$element",
];
```

### Classes

Dart 中的类的工作方式与其它面向对象语言中的类非常相似，但这里是你入门所需的大部分内容。

#### MEMBERS: PROPERTIES AND METHODS

属性是作为类成员声明的变量：

```
```dart
class User {
  int id;
  String username;
  String password;
}
```
```

方法是定义在类内部的函数。它们与顶层函数定义方式相同，但位于类定义的体内：

```
```dart
class User {
  int id;
  String username;
  String password;

  void setId(int newId) {
    id = newId;
  }
}
```
```

#### Accessing Members: Dot Notation

成员可以使用点记法访问。如果你想获取 user 对象的 id 属性并将其赋给一个变量，你会写：

```
```dart
var a = user.id;
```
```

并且你会像这样运行 setId 方法：

```
```dart
user.setId(1000)
```
```

#### Cascade Notation

如果你需要运行多个方法或更改对象的许多属性，可以使用 cascade 记法，它允许你写类似这样的内容，这将创建一个具有给定用户名、密码和 ID 的 User：

```
var a = User()
..username="yourUsername"
..password="ActuallyVeryComplicatedPass!"
..setId(1000);
```

#### CONSTRUCTORS

Dart 允许我们仅定义构造函数的签名，它将自动把参数赋给对应的属性并自动创建类的实例。参数的定义方式与任何函数的参数相同，但指向类的属性：

```
class ClassExample {
  ClassExample(
    this.var1, // 位置参数
    {
      @required this.var2, // 命名参数
      this.var3
    }
  );

  int var1;
  String var2;
  Function var3;
}
```

你可以按以下方式为继承自超类的类定义构造函数：

```
class ClassExample extends SuperClass {
  ClassExample(parameters) :
    super(superClassArguments);
}
```

#### Named Constructors

你也可以定义命名构造函数，它们是具有不同名称的替代构造函数：

```
class ClassExample {
  ClassExample(parameters);
  ClassExample.constructorName(otherParameters);
}
```

#### Regular Constructors

Dart 也支持具有自己主体的常规构造函数，也可以选择利用自动属性赋值。

这里有一个类的例子，它有一个构造函数接收一个整数值并将其平方赋给 var1，同时直接赋给 var2 和 var3：

```
class ClassExample {
  ClassExample(this.var1, this.var2, this.var3);
  ClassExample.withSqrtOfVar1(int var1sqrt, this.var2, this.var3) {
    var1 = var1sqrt*var1sqrt;
  }

  int var1;
  String var2;
  Function var3;
}
```

#### STATIC MEMBERS

静态类成员可以在无需实例化的情况下访问：

```
class ClassExample {
  // (...)

  static num square(num val) =>
    val*val;

  // (...)
}

// 与 C++ 不同，我们可以使用常规点记法
var n = ClassExample.square(0.1);

/*
 * 使用 == 比较浮点数
 * 在大多数编程语言中不可靠，
 * 包括 Dart，所以改用类似这样的语法
 */
assert(n < 0.011 && n > 0.009);
```

#### ABSTRACT CLASSES

抽象类是你不能实例化的类：你不能创建该类的对象。它们是通过用 abstract class 替换 class 关键字来声明的。抽象类通常用作混入（在 Writing a Class with a Mixin 中介绍）或接口（在 Writing a Class That implements an Interface 中介绍）。

抽象类可以包含抽象方法，即没有实现的方法，但也可以包含常规方法：

```
abstract class MyClass {
  void regularMethod() {
    // do something
  }

  void abstractMethod();
}
```

抽象方法可以由子类或实现 MyClass 接口的类实现，如以下各节所述。

#### SUBCLASSES, INTERFACES, AND MIXINS: EXTENDS, IMPLEMENTS, AND WITH

类可以通过三种方式相互交互：一个类可以是另一个类的子类，继承其所有成员并选择性地重写某些方法；它可以实现另一个类的接口；或它可以使用另一个类中定义的方法（称为 mixin）。

#### extends

尤其是在 Flutter 中，扩展另一个类是非常常见的。通过这样做我们创建了一个子类，我们扩展的类称为超类。我们可以决定声明新方法或重新实现（重写）现有方法。子类可以拥有自己的变量并通过 super 对象访问超类的变量。

#### @override

创建子类时，你可能想实现超类的一个方法，你可以通过用 @override 注解为前缀来做到这一点，就像我们在定义 widget 时对 build 方法所做的那样：

```
class MyWidget extends StatelessWidget {
  // (...)

  @override
  Widget build(BuildContext context) {
    // (...)
  }
}
```

该方法也可以定义为：

```
@override Widget build(context) {
  // (...)
}
```

有时你可能想通过在新方法定义开始处添加 super.methodName 来调用超类对应的方法，这样新方法就充当补充而不是替换，就像我们在编写 StatefulWidgetS 时重新定义 State 类的 initState 时必须做的那样：

```
class _MyWidgetState extends State<StatefulWidget> {
  // (...)

  @override
  void initState() {
    super.initState();
    // 我们自己的指令
  }

  // (...)
}
```

我们必须对 State 类这样做，因为它已在 Flutter widgets 库中用 @mustCallSuper 注解定义，并且如果我们想定义类以供子类创建并且某个重写方法应该调用超类的被重写方法，我们也可以这样做。

#### Writing a Class with a Mixin

Mixin 是一个包含供其他类使用的方法的类，而无需那些类通过扩展它并成为其子类直接继承 mixin。Mixin 可以简单地定义为一个类并通过声明任何想要使用它的类（应用混入）使用 mixin，就像以下示例中一样：

```
// the mixin
class MyMixin {
  String myMethod() => "Hi from the mixin";
}

class MyClass with MyMixin {

}

var obj = MyClass();

assert(obj.myMethod() == "Hi from the mixin");





### 编写实现接口的类

接口是一种只定义方法返回类型和参数的类，要求其他类实现这些方法。相应地，这通过使用`implements`关键字来完成：

```
abstract class MyInterface {
    abstract void myMethod();
}
```

```
class MyClass implements MyInterface {
    void myMethod() {
        // do something
    }
}

class MyOtherClass implements MyInterface {
    void myMethod() {
        // do something else
    }
}
```

完成上述操作后，你可以实例化并使用`MyClass`和`MyOtherClass`，并且可以放心地使用它们，因为它们实现了`MyInterface`中描述的所有方法。

### Typedef 与回调

typedef 用于创建回调类型，例如 VoidCallback，它是一种无参数函数的 void 回调类型。回调是一种闭包，作为参数提供给函数，在满足某些条件时被调用。闭包是一种定义方式很像变量的函数。VoidCallback 类型是一种特殊的 Function，它不接受参数且返回类型为 void，可以通过 typedef 按以下方式定义：

```
typedef VoidCallback = void Function();
```

如果你想定义一个接受某种参数的不同类型的回调，可以编写以下代码：

```
typedef IntFunctionCallback = int Function(int n);
```

或者下面这个非常常见的实变量实值函数的数学定义：

```
typedef RealFunctionOfARealVariable = double Function(double n);
```

定义可以是类型无关的（这种语法可以用于 Dart 中的任何类型）：

```
typedef OneArgumentCallback<T> = T Function(T a);
```

在定义了这样的类型或直接使用通用的 Function 类型之后，我们可以像创建变量一样创建函数。例如，如果我们想在 Dart 中重新实现 f(x) = e^x 指数函数（其中 e 是欧拉数，在某些国家更常被称为纳皮尔数），我们可以使用 dart:math 中的 pow 和 e 编写如下代码：

```
RealFunctionOfARealVariable f = (x) => pow(e, x);

assert(f(2) == pow(e, 2));
assert(f(1) > 2.71 && f(1) < 2.72);
```

### 掌握 import

Dart 中的 import 指令，与许多其他现代语言一样，不仅仅像老式的 C 语言 #include 那样处理外部库文件的包含：它还能做更多事情，让我们在处理库和包时更加轻松。

#### 基础知识

我们从基础知识开始：将整个文件导入到当前命名空间。

#### 导入本地文件

你可以使用相对路径导入另一个本地 Dart 文件：

```
import 'filename.dart';
```

#### 导入内置 Dart 库

你可以通过在库名称前加上 dart: 来导入内置 Dart 库：

```
import 'dart:async';
```

#### 从包中导入

你可以使用以下方式从包中导入文件：

```
import 'package:packagename/filename.dart';
```

通过使用 flutter 作为包名，可以用它从 Flutter SDK 导入文件：

```
import 'package:flutter/services.dart'
```

这种语法也可以用于导入应用 lib 目录中的文件，只需使用应用的包名作为包名。

#### 使用 as 在命名空间下导入

你还可以使用以下语法在命名空间下导入：

```
import 'customlib.dart' as MyCustomLib;
```

这样，例如，如果该文件中定义了 CustomLibraryClass 类，你可以通过 MyCustomLib.CustomLibraryClass 来访问它，而不只是 CustomLibraryClass。

这允许你导入许多库而不会使主命名空间变得杂乱，保持每个库相互独立，避免出现多个同名类的风险。

另一个很大的优势是，当你过一段时间重新查看代码时，你会确切知道该类来自哪个库，而无需先弄清楚这一点。

#### 使用 show 仅导入文件的部分内容

import 也可以用来只导入文件中的某些类。

例如，如果你只需要 customlib.dart 文件中的 CustomLibraryClass，而不想导入其他类（也许因为它们与命名空间中的其他类同名，而你不想使用 as 来处理额外的点号表示法），你可以使用以下代码：

```
import 'customlib.dart' show CustomLibraryClass;
```

### 原生 Java/Apple 与 Dart 数据类型之间的转换

| 类型 | Dart | Java | Objective-C/Swift |
|---|---|---|---|
| 整数 | `int` | Integer/Long（取决于大小） | NSNumber NumberWithInt:/NumberWithLong:（取决于大小） |
| 布尔值 | `bool` | Boolean | NSNumber NumberWithBool: |
| 字符串 | `String` | String | NSString |
| 一维且元素类型统一的集合 | `List` | ArrayList | NSArray |
| 键值对列表 | `Map` | HashMap | NSDictionary |

### 接下来去哪里

如果你在阅读本书正文之前先读了本附录，请回到开头并开始吧！本附录中的所有内容都将非常容易地应用于使用 Flutter 出色的声明式 UI 语法构建用户界面。

如果你已经读完了本书的其余部分，下一个附录将向你展示如何让应用在 iOS 上看起来像原生应用，以及如何使用平台特定的配置来设置自定义应用名称和应用图标，为应用分发做好准备。

脚注

[55] https://api.dartlang.org/stable/2.3.2/dart-core/List-class.html
