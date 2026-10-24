

```dart
] )
```

这一组件组合——虽然它只是一行（Row）组件——也能轻松插入到 ListView 或 GridView 中。沿用社交网络的例子，它可以作为社交网络的信息流。

这并不难做到——我们首先要让它符合组成它的各个元素所需的空间和尺寸：

```dart
Column(
  children: <Widget> [
    Row(
      children: <Widget> [
        CircleAvatar(
          backgroundImage: ProfilePicture(profileID),
        ),

        Padding(
          padding: EdgeInsets.all(20),
          child: Text(profileName),
        ),
      ],
    ),
    Align(
      alignment: FractionalOffset.centerLeft,
      child: Padding(
        padding: EdgeInsets.all(5),
        child: Text(postText),
      ),
    ),
    Divider(),
    Row(
      children: <Widget> [
        LikeButton(),
        CommentButton(),
        ShareButton()
      ],
    ),
  ],
)
```

剩下要做的就是将占位组件替换为真正的组件。

对于头像，我们将使用之前用过的同一个 `NetworkImage("https://pragprog.com/favicon.png")`，而点赞、评论和分享按钮则使用简单的 IconButton：

```dart
Column(
  children: <Widget> [
    Row(
      children: <Widget> [
        CircleAvatar(
          backgroundImage: NetworkImage(
            "https://pragprog.com/favicon.png",
          ),
        ),
        Padding(
          padding: EdgeInsets.all(20),
          child: Text("Short poster"),
        ),
      ],
    ),
    Align(
      alignment: FractionalOffset.centerLeft,
      child: Padding(
        padding: EdgeInsets.all(5),
        child: Text("This is a short post, but that doesn’t really matter"),
      ),
    ),
    Divider(),
    Row(
      children: <Widget> [
        IconButton(
          icon: Icon(Icons.thumb_up, color: Colors.redAccent),
          onPressed: () {},
        ),
        IconButton(
          icon: Icon(Icons.comment, color: Colors.greenAccent),
          onPressed: () {},
        ),
        IconButton(
          icon: Icon(Icons.share, color: Colors.blueAccent),
          onPressed: () {},
        ),
      ],
    ),
  ],
),
```

要创建包含帖子的完整 ListView 视图，我们还应该将帖子数据单独存储在一个 List 中，因为在真实应用中，你绝不会硬编码这类数据。

为此，我们首先创建一个类来保存帖子数据：

```dart
class Post {

  Post({this.profileName, this.profilePicture, this.postText});

  ImageProvider profilePicture;
  String profileName;
  String postText;
}
```

然后创建一个该类的 List，并填入一些数据：

```dart
List<Post> posts = [
  Post(
    postText: "This is a short post, but that doesn’t really matter",
    profileName: "Short poster",
    profilePicture: NetworkImage("https://pragprog.com/favicon.png"),
  ),
  Post(
    postText: "This is a longer post, which really shows how the " +
        "widgets will naturally expand when there is more content to display.\n" +
        "At this point, this line of text is just here because we need to " +
        "make this post so much bigger than the other one.",
    profileName: "Long poster",
    profilePicture: NetworkImage("https://pragprog.com/favicon.png"),
  ),
];
```

此时，生成的 ListView 就非常直观了：

```dart
ListView.builder(
  itemCount: 2,
  itemBuilder: (context, i) => Card(
    child: Column(
      children: <Widget> [
        Row(
          children: <Widget> [
            CircleAvatar(
              backgroundImage: posts[i].profilePicture,
            ),
            Padding(
              padding: EdgeInsets.all(20),
              child: Text(
                posts[i].profileName,
                style: Theme.of(context).textTheme.subhead,
              ),
            ),
          ],
        ),
        Align(
          alignment: FractionalOffset.centerLeft,
          child: Padding(
            padding: EdgeInsets.all(5),
            child: Text(
              posts[i].postText,
              style: Theme.of(context).textTheme.body1,
            ),
          ),
        ),
        Divider(),
        Row(
          children: <Widget> [
            IconButton(
              icon: Icon(Icons.thumb_up, color: Colors.redAccent),
              onPressed: () {},
            ),
            IconButton(
              icon: Icon(Icons.comment, color: Colors.greenAccent),
              onPressed: () {},
            ),
            IconButton(
              icon: Icon(Icons.share, color: Colors.blueAccent),
              onPressed: () {},
            ),
          ],
        ),
      ],
    ),
  ),
);
```

![](img/eb93f09b5f47b8f6c9bead1b9047c506_98_0.png)

嵌套的 Row 和 Column 也可以用来创建网格，如*嵌套的 Row 和 Column*中所述。

## 创建网格

根据你的应用需要实现的效果，可以通过两种不同的方式创建网格：使用 GridView，或使用嵌套的 Row 和 Column。

### GridView

GridView 之于网格，就像 ListView 之于垂直布局一样。它适用于类似照片应用中那种网格：由数量可变的固定大小元素组成的长型可滚动网格。GridView 与 ListView 非常相似，也可以使用多种构造函数来创建。默认构造函数将在*Slivers、自定义可滚动组件和可折叠应用栏*一节中讨论。

#### 固定交叉轴数量的网格

GridView.count 构造函数以 children Widgets 的 List 和 crossAxisCount 作为构建网格的基本参数，如下例所示：

```dart
GridView.count(
  crossAxisCount: 3,
  crossAxisSpacing: 50.0,
  mainAxisSpacing: 100.0,
  padding: EdgeInsets.all(20.0),
  children: [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]
      .map(
        (n) =>
          Text(
            "$n",
            style: Theme.of(context).textTheme.display1,
          ),
      ).toList(),
)
```

生成的效果如下：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_100_0.png)

#### 每组件交叉轴最大范围的网格

GridView.extent 构造函数使用 children Widgets 的 List 和 maxCrossAxisExtent 作为参数。maxCrossAxisExtent 是每个子组件在交叉轴上可以占用的最大像素数。maxCrossAxisExtent 越大，每个网格单元就越大。

例如，下面的代码：

```dart
GridView.extent(
  maxCrossAxisExtent: 50.0,
  crossAxisSpacing: 50.0,
  mainAxisSpacing: 100.0,
  padding: EdgeInsets.all(20.0),
  children: [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]
    .map(
      (n) =>
        Text(
          "$n",
          style: Theme.of(context).textTheme.display1,
        ),
    ).toList(),
)
```

在 2880x1440、560dpi、使用 Android 默认缩放的屏幕上，会生成下面这样的网格：

![](img/eb93f09b5f47b8f6c9bead1b9047c506_101_0.png)

#### 使用 Builder 函数构建网格

GridView.builder 构造函数使用 itemBuilder 来构建子组件，并使用 gridDelegate 来控制组件的大小和位置。下面的示例生成的网格与前面的 GridView.count 示例（此处）相同：

```dart
GridView.builder(
  padding: EdgeInsets.all(20.0),
  itemCount: 15,
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 3,
    crossAxisSpacing: 50.0,
    mainAxisSpacing: 100.0,
  ),
  itemBuilder: (context, i) => Text("${i+1}"),
)
```

网格委托（grid delegate）有着（异常长的）名称，例如 SliverGridDelegateWithFixedCrossAxisCount 和 SliverGridDelegateWithMaxCrossAxisExtent，它们分别对应 GridView.count 和 GridView.extent 的属性。它们以命名参数的形式接收与网格布局相关的属性。

如果你想精确复现这里展示的 GridView.extent 的行为，可以编写如下代码：

```dart
GridView.builder(
  padding: EdgeInsets.all(20.0),
  itemCount: 15,
  gridDelegate: SliverGridDelegateWithMaxCrossAxisExtent(
    maxCrossAxisExtent: 50.0,
    crossAxisSpacing: 50.0,
    mainAxisSpacing: 100.0,
  ),
  itemBuilder: (context, i) => Text("${i+1}"),
)
```

#### GridView 的属性

其他网格属性包括：

- childAspectRatio，指网格中每个子组件在交叉轴与主轴方向上的尺寸之比。
- shrinkWrap，与 ListView 的 shrinkWrap 相同（见*如何使用*）。
- scrollDirection，设置网格滚动的方向（同时也是网格的主轴方向）。

### 嵌套的 Row 和 Column

如果你需要创建一个（可能较小的）不可滚动的、包含固定数量组件的网格，那么可以使用由 Row 组成的 Column。

一个简单的 2x2 网格可以这样实现：

```dart
Column(
  children: <Widget> [
    Row(
      children: <Widget> [
        Text(topLeftString),
        Text(topRightString),
      ],
    ),
    Row(
      children: <Widget> [
        Text(bottomLeftString),
        Text(bottomRightString),
      ],
    ),
  ],
)
```

