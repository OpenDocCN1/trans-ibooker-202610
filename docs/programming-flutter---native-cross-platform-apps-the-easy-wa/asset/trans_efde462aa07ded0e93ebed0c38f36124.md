

## 显示图像

图像可以有不同的用途，也可能来自不同的地方，因此在 Flutter 中显示图像的方式取决于你想从哪里加载图像以及你需要将它们用于什么目的。

有两个类用于此目的。一个是 Image，它是一个组件，就像 Text、Icon 或你目前见过的任何其他组件一样。另一个是 ImageProvider，当你需要指定一个图像并对外部操纵其大小或形状时，需要用到它。

Image 组件是图像的表示形式，你可以将其添加到应用布局中的任意位置；而 ImageProvider 类是图像的来源，它用来与该图像一起构成组件。

## 从网络显示图像

如果你有一个包含图像的 URL，形式为 `http://example.com/image.png`，你可以通过使用 Image.network 构造器来获取对应的 Image 组件，如下所示：

```
Image.network("http://example.com/image.png")
```

你可以像使用任何其他组件（如 Text 或 Icon）一样在任意位置使用它。

## 更改图像的属性

你可以更改以下选项：

- scale，是一个 double 值，你可以用它来设置图像显示时的缩放比例。
- 最大宽度和高度（以像素为单位）。
- repeat，控制图像是否会在宽度和高度范围内重复，类型为 ImageRepeat。默认情况下，ImageRepeat 设置为 ImageRepeat.noRepeat，这会使剩余的宽度和高度保持为空或透明。其他选项包括 ImageRepeat.repeatX 和 ImageRepeat.repeatY（它们控制图像是只能沿一个轴重复还是沿另一个轴重复）以及 ImageRepeat.repeat（它使图像能够在所有方向上重复以适配宽度和高度）。
- 组件的背景颜色，在存在透明区域的地方显示。如果宽度和高度与图像的不兼容且不允许重复，它也会显示在图像周围。
- headers 使用 Map<String, String> 设置 HTTP 请求的请求头。
- 用于屏幕阅读辅助功能服务的 semanticLabel 字符串，以及当明确不需要时使用的 excludeFromSemantics 布尔值。

## 使用宽度、高度和重复属性按你需要的方式显示图像

为了进一步说明 width、height 和 repeat 的用法，我们将看一些屏幕截图，它们展示了大小和重复设置的不同组合。

如果你完全不设置任何内容，只给定一个 URL，像这样：

```
Image.network(
  url
)
```

图像将适配视图，如你在第一张屏幕截图中所见。

![](img/eb93f09b5f47b8f6c9bead1b9047c506_71_0.png)

如果你设置的宽度小于视图的宽度，像这样：

```
Image.network(
  url,
  width: 200.0
)
```

图像将按比例缩小以适应该宽度（见第二张屏幕截图）。

![](img/eb93f09b5f47b8f6c9bead1b9047c506_72_0.png)

设置大于视图宽度的宽度将不会产生任何效果：结果将与第一张屏幕截图相同。设置高于或低于图像在正常情况下最终显示的高度（考虑到在这种情况下图像宽度受限），其效果与之前看到的将宽度设置为大于或小于视图宽度时的效果相同。

当我们将 *repeat* 设置为除默认值 `ImageRepeat.noRepeat` 之外的任何值时，情况就会发生变化；允许重复可以填充与我们要显示的图像宽高比不同的宽度/高度比例。

如果我们同时设置了宽度和高度，图像将在超出原始图像宽高比的那一侧重复——例如，如果你将高度设置为较高的值、宽度设置为较低的值，并将重复设置为 `ImageRepeat.repeatX` 或 `ImageRepeat.repeatY`，以下代码：

```
Image.network(
    url,
    width: 412.0,
    height: 450.0,
    repeat: ImageRepeat.repeatY // or ImageRepeat.repeat
)
```

将产生第一张屏幕截图所示的结果，图像在垂直方向上重复以适配更高的高度。

![](img/eb93f09b5f47b8f6c9bead1b9047c506_73_0.png)

相反，如果你设置的高度和宽度使图像在水平方向上有多余的空间（通过进一步限制高度实现），像这个示例一样：

```
Image.network(
url,
width: 380.0,
height: 180.0,
repeat: ImageRepeat.repeatX // or ImageRepeat.repeat
)
```

你将得到第二张屏幕截图所示的结果。

![](img/eb93f09b5f47b8f6c9bead1b9047c506_73_1.png)

如果我们只是限制高度，结果会非常相似，但图像会填满整个可用的水平空间，在重复的部分中显示图像更大的部分。

**用于网络图像的 ImageProvider**

当我们希望将来自网络的图像用于 **ImageProvider** 时（例如，在 _制作圆形组件_ 中讨论如何制作圆形组件时就会用到），我们需要使用 **NetworkImage** 类。通过将图像的 URL 传递给其构造器来创建 **NetworkImage**：

```
var imageProv = NetworkImage("http://example.com/image.png")
```

**NetworkImage** 构造器还允许设置图像的缩放比例以及图像 GET 请求的 HTTP 请求头。

## 从资源显示图像

我们在 _向应用添加你自己的资源_ 中学习了如何向 Flutter 应用添加资源，但现在你将看到那些用于获取所添加图像并显示它们的类。

给定资源的名称，你可以通过以下方式显示该图像资源：

```
Image.asset(
  "imagename.png",
)
```

**用于资源图像的 ImageProvider**

你可以使用 **AssetImage** 类将图像用作 **ImageProvider**：

```
AssetImage(
  "imagename.png",
)
```

## 从内存显示图像

当图像使用 **Image.toByteData()** 以字节形式存储在内存中时，会使用这种方式，它会将它们存储为 **Uint8List**（我们将在 _字节列表及其他：dart:typed_data 库_ 中进一步讨论）。例如，要将名为 **image** 的图像存储到名为 **bytes** 的变量中，你可以使用以下代码：

```
var bytes = image.toByteData();
```

完成之后，你可以将其写入文件，或者直接将其保留在内存中。

如果你想再次将这些字节作为图像显示，可以编写以下代码：

```
Image.memory(
  bytes
)
```

## 用于内存图像的 ImageProvider

你可以使用 **MemoryImage** 类将存储在内存中的图像用作 ImageProvider，正如你可能已经预料到的，它的用法如下：

```
MemoryImage(
  bytes
)
```

## 在 Image 组件中使用 ImageProvider

作为示例，我们将使用 ImageProvider 创建 Image 组件，即将 ImageProvider 作为命名参数 image 传入。在这种情况下，这增加了不必要的复杂性，但理解如何创建 ImageProvider 对于理解本书中的其他示例非常重要：

```
Image(
  image: NetworkImage(
    "http://example.com/image.png"
  ),
)
```

以及任何缩放、宽度、高度、重复设置或其他对图像的修改。

对于 AssetImage 和 MemoryImage 也是如此，这可能有助于解释 ImageProvider 与 Image 组件相比所包含数据的不同。

## 垂直布局

当开发者需要垂直排列项目时，根据需求有两种选择：Column 和 ListView。

### COLUMN（列）

Column 用于创建组件的垂直堆叠，并且不可滚动。它用于在垂直方向上一个接一个地显示项目，而不是创建项目列表。

**如何使用它**

你可以通过以下方式创建一个简单的 Column：

```
Column(
  children: <Widget> [
    // Widgets separated by commas
  ],
)
```

**布局组件选项**

你只需要将组件放在 Widget 集合字面量的方括号内（更多关于集合字面量和列表的内容，请参见 _列表和集合字面量_），组件就会以垂直堆叠的方式显示，但构造器参数中有很多选项可以自定义，比如 mainAxisAlignment，它设置子组件在主轴上如何对齐（对于 Column 来说主轴是垂直轴），它可以是以下值之一：

- *MainAxisAlignment.start*、*MainAxisAlignment.center* 或 *MainAxisAlignment.end*，分别将组件尽可能靠近主轴的开头、中间或结尾放置。
- *MainAxisAlignment.spaceAround*，使用可用空间的一半来分隔子组件彼此，剩下的另一半用于将第一个和最后一个子组件与 Column 的开头和结尾分隔开。
- *MainAxisAlignment.spaceBetween*，使用空间使子组件彼此尽可能远离。
- *MainAxisAlignment.spaceEvenly*，使每个子组件与下一个子组件之间的间距，与 Column 开头到第一个子组件以及最后一个子组件到 Column 末尾的间距相同。

