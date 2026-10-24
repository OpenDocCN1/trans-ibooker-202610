

## 在 Flutter 中定义组件

```
class MyApp extends StatelessWidget
```

在 Flutter 中，每个 UI 元素或 UI 元素集合都是一个组件（widget），它是一个不可变对象。这意味着我们不能更改组件的成员变量：如果有任何成员变量，它们必须声明为 final 或 constant。关于 Dart 变量的更多信息，请参阅《变量与条件》。

组件有两种类型：

- 无状态组件（StatelessWidget 的子类），在应用启动时以及父组件被重新渲染时渲染，例如 MyApp。
- 有状态组件（StatefulWidget 的子类），你可以随时通过调用 setState() 函数来重新渲染它，例如本应用中的 MyHomePage，这将在《主页组件》中介绍。

应用组件是所有其他组件的集合，因此它也是一个组件。在这里，它是一个 StatelessWidget。

## 使用无状态组件

firstapp_starting/lib/main.dart

```
@override
Widget build(BuildContext context) {
  return MaterialApp(
    title: 'Flutter Demo',
    theme: ThemeData(
      primarySwatch: Colors.blue,
    ),
    home: MyHomePage(title: 'Flutter Demo Home Page'),
  );
}
```

这是 build 方法，在组件需要被渲染时调用，并返回一个 Widget 对象，它是所有组件的超类。

在本例中，它只被调用一次：当应用启动时，因为 MyApp 没有父组件（它是所有其他组件的父组件）。

在 MyApp 的 build 方法中，指定了 MyApp 组件（应用本身）是一个 MaterialApp，这意味着我们的应用将使用标准库中提供的 Material Design 功能。

代码通过 MaterialApp 构造函数设置了应用标题、带有主色板（一组相近的颜色，如 Google 的 Material Design 指南中所规定）的主题，以及一个主页，该主页的类型为 MyHomePage，标题为“Flutter Demo Home Page”。

## 主页组件

这里定义了前面提到的 MyHomePage 类，它是一个 StatefulWidget（有状态的组件，不同于 MyApp），带有一个 title 成员和一个状态，即 _MyHomePageState：

firstapp_starting/lib/main.dart

```
class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}
```

在 Dart 中，_MyHomePageState 中的下划线（_）具有类似 Java 等语言中 private 的效果，这一点在《下划线》中有解释。

## 有状态组件的优势

有状态组件具备以编程方式重新渲染的能力，使开发者能够构建对正在显示或表示的数据变化做出反应的应用。

因此，它们比无状态组件更复杂：有状态组件有一个状态，由一个 State 对象来表示，该对象包含组件的逻辑和可变数据。正如我在上一节提到的，组件本身是不可变的，因此它们需要另一个对象（State）来持有可变数据。

当你调用 setState() 函数时，它们会被重新渲染，该函数会通知框架组件的状态已更改，需要通过再次调用其 build 方法重新渲染，以向用户显示更改。

## MyHomePage 组件

这一行：

```
MyHomePage({Key key, this.title}) : super(key: key);
```

定义了 MyHomePage 的参数（如《构造函数》中所述）。由于它们被包裹在大括号中，因此它们是命名参数，这就是为什么在调用 MyHomePage 的构造函数时语法是这样的：

```
MyHomePage(title: 'Flutter Demo Home Page')
```

未使用的 key 参数被框架用来识别哪些组件需要重新渲染。你可以提供它，它可能很有用，但它是可选的，对于本应用我们不需要它。

我们将在下一章的《Key》中讨论 key。

关于 Dart 类和构造函数定义的更多信息，请参阅《类》。

_MyHomePageState 内部定义了应用的逻辑：

firstapp_starting/lib/main.dart

```
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'You have pushed the button this many times:',
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```

## 使用组件提供基本应用结构：Scaffold

这次我们返回的组件是一个 Scaffold，即基本的 Material Design 可视化布局结构；换句话说，它是我们所有应用内容的通用容器。

一个 Scaffold 有一个 AppBar，它（使用 Text 组件）显示之前在 MyHomePage 构造函数中设置的标题。

## 构建应用主体

我们的 Scaffold 的主体被包裹在一个 Center 组件中，它将我们的内容居中于页面中间。

Center 的子组件（它居中的内容）是一个 Column，它以垂直堆叠的方式显示其子组件。

第一个 Text 组件只是显示了一个字符串，而第二个 Text 组件显示计数器，并使用了一个名为 display1 的内置主题。

## 添加浮动操作按钮

不在居中位置（但在 Scaffold 内部，位于主体之外，在其自己的 floatingActionButton 属性中）的是 FloatingActionButton，按下时会调用我们之前看到的 _incrementCounter 函数。

很容易理解的是 tooltip 代码和其中的 Icon 组件（再次强调，这里一切都是组件），它是一个简单的加号（或称为“add”）图标。

每次我们点击 FAB 时，计数器都会递增，状态发生改变，setState() 调用会再次触发 build 方法，UI 会使用更新后的计数器重新加载。

## 我们已看到的以及接下来要去向哪里

这是一个非常简单的应用：它只是一个 MaterialApp，其中包含一个 Scaffold，右下角有一个简单的 FAB，中间是一个 Column，包含两个 Text 组件，显示 FAB 被点击的次数。

这非常简单，但真正理解某件事如何实现的唯一方法就是亲自去做，因此下一节将专注于为现有代码添加新功能。

## 为应用添加个性化设置

现在，让我们修改一下我们已看到的内容。我们将把应用的颜色改为绿色，添加一个红色重置按钮将计数器重置为 0，让应用在计数器为 0 时显示字符串“None”而不是计数器数值，并更改 appBar 的标题文本。这样，我们就能体会到如何进行简单的更改，同时也能初步了解更复杂的 Flutter 语法。

结果将如下所示：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_45_0.png)

## 更改应用的主色

我们开始编辑应用，先做最简单的更改：将颜色改为绿色。只需更改 MyApp build 方法中的以下几行：

firstapp_starting/lib/main.dart

```
theme: ThemeData(
  primarySwatch: Colors.blue,
),
```

继续操作，将 Colors.blue 改为 Colors.green，使这几行变成如下所示：

firstapp/lib/main.dart

```
theme: ThemeData(
  primarySwatch: Colors.green,
),
```

## 将计数器显示为“None”而不是 0

现在，让我们更改中间的计数器文本，使其显示“None”而不是 0。

为此，我们将在 _MyHomePageState 中添加一个 _displayedString 变量，位置在计数器声明下方。

找到下面这一行：

```
int _counter = 0;
```

然后在它下面添加：

```
String _displayedString;
```

完成之后，我们需要在计数器为 0 时将变量设为“None”，否则设为计数器的字符串表示形式。这需要使用条件判断。

## 在 Flutter 中使用条件判断

为了在每次计数器增加时都这样做（我们通过调用 setState 来触发重新渲染，从而调用 build 方法），这需要成为 _MyHomePageState 的 build 方法的一部分。因此，在下面这行代码的上方：

```
return new Scaffold(
```

添加：

firstapp/lib/main.dart

```
if (_counter == 0) {
  _displayedString = "None";
} else {
  _displayedString = _counter.toString();
}
```

