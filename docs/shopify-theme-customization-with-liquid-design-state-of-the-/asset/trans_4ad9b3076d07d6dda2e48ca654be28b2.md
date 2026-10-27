

我们还可以加入一些额外的文本输入选项，以便创建多个公告，但这需要我们在每次需要添加额外公告时手动编辑 JSON 代码。*如果我们想要创建一个部分，可以在不每次都修改 JSON 代码的情况下添加任意数量的公告，该怎么办？* 对于这种情况，我们可以使用 `blocks` 属性。

## 使用 blocks 构建

`blocks` 属性是 Shopify 中最强大的工具之一。通过使用它，我们可以创建可重复使用任意次数的模块，并从主题编辑器中重新排列部分内容。此时，这可能会让人感到困惑，因为它听起来与我们刚刚学到的关于动态部分的内容相似。然而，关键区别在于 `blocks` 部分允许我们在部分内部重新排列内容，而不是部分本身，这使我们能够创建更复杂的功能。

此外，我们可以将 `blocks` 属性与静态部分结合起来，创建一个动态部分功能，其效果类似于我们当前在主页上看到的，然后将其包含在任何页面中。但是，我们将重新排列的是 blocks，而不是部分。

`blocks` 属性允许我们使用对象格式创建不同类型的 blocks，在此格式下，每个对象类型将充当一个独特的模块。在这里，我们可以在这些模块中的每一个下包含不同的输入设置选项。

以下是创建和使用 `blocks` 模块的步骤：

- 1. 让我们学习如何通过引入 `blocks` 属性来修改之前创建的公告栏部分。此外，我们将删除某些功能，例如翻译键和 `class` 属性，以便代码保持简洁易读：

```
{ % schema % }
{
  "name": "Announcement Bar",
  "settings": [
    {
      "type": "text",
      "id": "announcement-text",
      "label": "Text"
    },
```
```
{
    "type": "color",
    "id": "announcement-text-color",
    "label": "Text color",
    "default": "#000000"
}
],
"blocks": [
],
"presets": [
    {
        "name": "Announcement bar",
        "category": "Text"
    }
]
}
{% endschema %}
```

像创建部分一样，blocks 属性有一组自己的属性，我们需要使用它们来创建不同的模块：

- - name 属性允许我们设置 blocks 模块的名称，并决定该 block 在主题编辑器中如何显示。name 属性是强制性的。
- - type 属性是强制性属性，接受字符串值，我们可以在其中定义 block 类型。请注意，type 属性没有预定义的值集。相反，我们可以使用任何字符串值来定义 block 类型。
- - 使用 limit 属性，我们可以限制特定 block 类型可以重复的次数。limit 属性是可选的，仅接受数字类型值。
- - settings 属性允许我们包含 blocks 模块特定的设置。settings 属性是可选的。

请注意，blocks 模块的 name 和 type 属性需要在部分内保持唯一，而 id 属性需要在 blocks 模块内保持唯一。否则，我们将得到无效的 JSON 代码。

在 blocks 模块内使用文本输入后，我们可以动态重复整个 block 最多三次，从而创建三个单独的公告。

4. 让我们通过导航到主题编辑器并点击 Add section 按钮，在主页上包含公告栏部分来看看这是什么样子的。我们会注意到的第一件事是 Text color 选项，我们将其保留在 section settings 对象内：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_253_0.png)

图 7.3 – 主题编辑器中部分的示例

由于我们仅迁移了文本输入，并将文本颜色输入保留在 section settings 对象内，因此我们可以使用它一次为所有 blocks 设置文本颜色。让我们点击公告栏标题左侧的箭头返回到上一个部分，看看我们的 blocks 模块的实际效果。

我们会注意到，除了 Add section 按钮外，我们还在创建的公告栏部分下还有一个 Add Announcement 按钮：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_253_1.png)

图 7.4 – 部分 block 模块的示例

5. 点击 Announcement Bar 将把我们带回上一页，在那里我们可以配置 section settings 对象中的设置，而点击 Add Announcement 按钮将自动包含一个 blocks 模块并立即将我们定位到该 block 中。在这里，我们可以定义公告文本输入。

- 6. 在定义 `blocks` 模块对象设置中的所有输入后，我们可以点击 **Announcement** 标题左侧的箭头返回到上一页，在那里我们可以包含额外的 `blocks` 模块。但是，请注意，由于我们引入了值为 3 的 `limit` 属性，因此我们只能重复 **Announcement** block 最多 3 次，如下所示：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_254_0.png)

图 7.5 – 部分 block 模块数量有限的示例

一旦我们按照定义的值包含了最大数量的模块，**Add Announcement** 按钮将变灰，表明我们已达到 `blocks` 模块的最大数量。同样，与动态部分一样，将鼠标悬停在 `blocks` 模块上会显示两个图标。这些图标允许我们隐藏当前选定的 block 或使用拖放功能重新排列 block 的顺序。

现在我们已经学会了如何创建和使用 blocks 模块，我们需要学习如何输出 blocks 模块输入设置的 values：

- 1. 我们可以像访问 `section` 对象一样访问 `blocks` 模块的对象设置：我们需要使用 `section` 对象结合 `blocks` 属性。此 `section` 对象与 `blocks` 属性的组合将返回一个 sections blocks 数组，我们可以使用简单的 for 循环访问它：

```
{% for block in section.blocks %}
{% endfor %}
```

- 2. 一旦我们创建了 `for` 循环，剩下的唯一要做的就是输出每个 block 的值，就像我们对 sections 所做的那样。唯一的区别是，这次我们将使用在 `for` 循环中定义的变量，而不是使用 `sections` 关键字：

```
```liquid
{% for block in section.blocks %}
  {{ block.settings.announcement-text }}
{% endfor %}
```
```

到目前为止，我们已经学习了如何创建整个 section schema，通过它我们可以创建静态和动态部分并构建 blocks 模块。我们还学习了如何输出 `sections` 和 `blocks` 模块输入设置的 values。然而，在之前的示例中，我们只有一个类型的 block；*如果我们有多个 block 类型怎么办？*

section blocks 的真正强大之处在于，我们可以在单个 section 元素内创建多个 blocks 模块类型，我们可以通过简单地创建具有不同 type 值的多个 section blocks 对象来实现：

```
```json
"blocks": [
  {
    "name": "Product",
    "type": "product",
    "settings": [
      {
        "type": "product",
        "id": "featured-product",
        "label": "Product"
      }
    ]
  },
  {
    "name": "Collection",
    "type": "collection",
    "settings": [
      {
        "type": "collection",
        "id": "featured-collection",
        "label": "collection"
```
```
}
]
}
]
```

请注意，所有 name 和 type 属性值需要在整个 section 中保持唯一，而输入设置的 id 属性仅需要在单个 block 内保持唯一。

正如我们所见，创建多个 blocks 模块元素相对简单。虽然我们在示例中仅创建了两个简单的 blocks 模块，但我们可以创建任意数量的 blocks 模块，并通过主题编辑器重新排列它们以创建复杂的布局：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_256_0.png)

图 7.6 – 部分内多个 blocks 模块的示例

正如我们从示例中看到的，我们有四种不同类型的 blocks（**Text、Collection、Image 和 Video**），我们可以使用它们来创建商店所有者可以轻松配置的布局，而无需自行修改代码。

