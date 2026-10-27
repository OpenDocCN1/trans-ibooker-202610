

如前所述，在 Shopify Unite 2021 活动之前，文章输入值返回的是句柄字符串。要访问文章对象，我们需要将所访问对象名称变为复数形式，然后加上方括号 `[]` 表示法，这与我们在第 2 章“Liquid 的基本流程”中对产品句柄所做的操作类似：

```liquid
{% assign article = articles[settings.featured_article] %}
```

我们现在可以访问文章对象，并轻松地将任何类型的文章内容输出到商店的任何位置。请注意，虽然这两种方法产生的结果相同，但通过句柄恢复与页面相关的输入对象的第二种方法现已弃用。

## 博客输入

博客类型输入的工作方式与之类似，因为它为我们提供了一个特殊的博客选择器字段，通过该字段我们可以访问商店中所有可用的文章：

```json
{
  "type": "blog",
  "id": "featured_blog",
  "label": "Featured blog"
}
```

访问特色博客时，如果尚未定义该值，则始终返回博客对象或 nil：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_228_0.png)

图 6.21 – 博客专用输入类型示例

与文章对象类似，我们可以直接访问博客对象，或使用方括号 `[]` 表示法来检索它。唯一的区别是对象关键字发生了变化：

```liquid
{% assign blog = blogs[settings.featured_blog] %}
{% for article in blog %}
{% endfor %}
```

一旦我们恢复了博客对象，就可以使用 `for` 标签遍历所选博客中的所有文章，并正确渲染它们。

## 集合输入

集合类型输入为我们提供了一个特殊的集合选择器字段，通过该字段我们可以访问所选集合中的所有可用产品：

```json
{
  "type": "collection",
  "id": "featured_collection",
  "label": "Featured Collection"
}
```

与之前的设置类型类似，如果尚未选择集合，则返回值为 nil。否则，返回值将是一个对象值，我们可以用它来检索任何对象属性值：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_229_0.png)

图 6.22 – 集合专用输入类型示例

此外，我们可以直接访问 `collection` 对象，或者通过将对象名称变为复数形式，然后加上方括号 `[]` 表示法来访问：

```liquid
{% assign collection =
collections[settings.featured_collection] %}
```

有了集合对象，我们现在可以轻松地访问所有集合属性。

## 页面输入

页面类型输入为我们提供了另一个特殊的选择器字段，允许我们访问在商店后台的 **页面** 部分中预先创建的所有页面：

```json
{
  "type": "page",
  "id": "featured_page",
  "label": "Featured page"
}
```

访问 **特色页面** 值时，如果尚未定义该值，则始终返回对象值或 nil：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_230_0.png)

图 6.23 – 页面专用输入类型示例

我们可以直接访问页面对象，或通过方括号 `[]` 表示法访问：

```liquid
{% assign page = pages[settings.featured_page] %}
```

使用声明的页面变量，我们现在可以访问页面对象，并由此进一步访问特定页面的任何属性。

## 产品输入

顾名思义，product 为我们提供了一个产品输入选择器，我们可以用它来访问产品对象：

```json
{
  "type": "product",
  "id": "featured_product",
  "label": "Featured product"
}
```

与之前的设置类似，访问时，如果尚未定义该值，则返回 nil，否则我们会得到一个对象值：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_231_0.png)

图 6.24 – 产品专用输入类型示例

我们可以使用返回的对象值来访问属性值，或者使用产品对象的方括号 `[]` 表示法通过已弃用的方法来检索对象：

```liquid
{% assign product = products[settings.featured_product] %}
```

通过产品变量，我们现在可以访问所选产品对象中的所有属性。

到目前为止，我们已经了解了所有不同类型的输入设置，无论是基本输入类型还是专用输入类型，如何访问它们，每种设置可以包含哪些额外属性，以及我们可以期望从它们那里获得什么类型的值。然而，如果这些设置没有顺序，简单地在单个类别中堆叠大量设置可能会令人不知所措。

## 组织主题编辑器

在上一节中，我们学习了不同类型的可配置设置，这些设置使我们能够通过一系列基本或专用输入选择器轻松更新其值。现在我们将学习一组不同的设置：侧边栏设置。侧边栏设置将允许我们将每个类别中的设置划分到其他单独的块中。

侧边栏设置不持有任何值，也无法通过主题编辑器进行配置。它们的唯一用途是为我们提供附加信息，并帮助我们将不同的输入设置组组织成单独的块，以便更直接地导航。

在侧边栏设置下，我们可以使用以下选项：

- `header`
- `paragraph`

与基本和专用输入设置不同，`header` 和 `paragraph` 只能包含以下标准属性：

- 顾名思义，`type` 属性允许我们设置设置类型，其值可以是 `header` 或 `paragraph` 设置。对于 `header` 和 `paragraph`，`type` 属性都是必需的。
- 虽然侧边栏设置不能包含任何值，但我们可以使用 `content` 属性向主题编辑器输出特定信息。对于 `header` 和 `paragraph`，`content` 属性同样是必需的。
- 最后一个属性 `info` 允许我们提供有关 `header` 类型设置的附加信息。请注意，`info` 不是必需属性，我们只能将其用于 `header` 类型设置。

现在我们已经掌握了有关不同类型的侧边栏设置及其属性的所有必要信息，让我们分别查看每种设置类型并学习如何使用它们。

### header 类型

`header` 类型设置，顾名思义，允许我们创建一个 header 元素，从而将所有设置输入类型分组到单个块内。请注意，引入 `header` 类型设置后，它会自动将该类别中的所有输入类型设置分组，直到遇到另一个 `header` 元素，或者当前类别中没有更多设置为止：

```json
{
  "type": "header",
  "content": "Newsletter settings",
  "info": "Enabling the popup feature will automatically trigger a newsletter popup on page load."
}
```

正如我们所见，使用 header 类型设置相对容易，而且我们获得的好处非常显著，因为现在我们可以轻松地对相关的输入设置进行分组。此外，通过包含可选的 `info` 属性，我们能够添加与该特定设置块相关的一些附加信息，如下面的截图所示：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_233_0.png)

图 6.25 – header 侧边栏设置类型示例

### paragraph 类型

虽然我们不能包含额外属性，但我们可以使用 `content` 属性创建指向特定页面的直接链接，从而提供一些附加信息。我们可以通过以下格式包含所需信息来实现这一点：

```text
[link text](link url)
```

包含指向特定页面的直接链接将使我们的主题编辑器更加简洁。同时，它还允许我们包含使用它的人可能需要的所有必要信息：

```json
{
  "type": "paragraph",
  "content": "The paragraph type setting allows us to include a direct link to any page. [Learn more](https://shopify.dev/themes/architecture/settings/sidebar-settings#header)"
}
```

请注意，我们可以在任何接受 `info` 属性的设置类型中，于 `content` 和 `info` 属性内创建指向任何页面的直接链接。

现在我们已经了解了每种输入设置和侧边栏设置类型。然而，了解所有当前设置固然重要，但提及一些现已弃用但我们日常工作中仍可能遇到的设置也同样重要。

## 一瞥已弃用的设置

虽然以下设置已不再受支持，但我们很可能在商店所有者从未更新过的旧主题上遇到它们。由于这些设置不再受支持，我们将不会详细讨论它们的工作原理，但我们会提供一些一般性指导，以帮助识别它们及其作用。

### 字体输入

`font` 输入类型设置允许我们生成 Shopify 上可访问的字体文件候选列表：

```json
{
  "type": "font",
  "id": "body_font",
  "label": "Body font"
}
```

