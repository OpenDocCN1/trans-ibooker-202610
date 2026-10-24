
那些类。StarredPage 的 fetchComic 仅在 _retrieveSavedComics 内部调用，而 HomeScreen 的 fetchComic 则被设置为 build 方法中 FutureBuilder 的 future。

## 测试 fetchComic

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

## 将测试分组

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

## 浅谈更复杂应用的 Widget 测试

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

## 定义 TextEditingController

`TextEditingController` 是一个类，用于存储插入到 `TextField` 或类似 Widget 中的数据，并使我们能够从赋给 `TextField` 本身的 `onSubmitted` 回调之外访问这些数据，从而允许我们创建一个打开用户所请求漫画的按钮，并在执行集成测试时为我们提供可点击的对象。

让我们将 `TextEditingController` 声明为 `SelectionPage` 的私有成员变量：

```
final TextEditingController _controller = TextEditingController();
```

## 使用 TextEditingController

让我们将它作为 `controller` 参数传递给 `TextField`：

```
TextField(
  controller: _controller,
  decoration: InputDecoration(
    labelText: "Insert comic #",
  ),
)
```

## 添加 FlatButton

我们现在需要将 `Scaffold` 的 `body` 中的 `Center` Widget 转换为 `Column`，这还需要将 `child` 参数改为 `children`，而 `children` 需要是一个 `List` 字面量（我们需要将 `TextField` 定义放在方括号中，并在其中添加 `FlatButton`）。

我们要添加的 `FlatButton` 需要一个在集成测试中使用的 `key`，并且它需要执行 `TextField` 的 `onSubmit` 所执行的相同操作，但通过获取
