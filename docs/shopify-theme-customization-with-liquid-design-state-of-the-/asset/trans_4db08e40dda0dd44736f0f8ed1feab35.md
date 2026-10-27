

以下代码展示了带有前置属性的 `video_url` 类型输入：

```
{
  "type": "video_url",
  "id": "banner_video_url",
  "label": "Video url",
  "accept": [
    "youtube",
    "vimeo"
  ],
  "placeholder": "Enter the YouTube or Vimeo video URL."
}
```

在将视频 URL 添加到两个视频平台之一后，我们将看到视频名称和第一帧，从而可以确认我们拥有正确的视频 URL：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_219_0.png)

图 6.17 – `video_url` 专用输入类型的示例

Files 部分是我们通过管理员或主题编辑器上传的所有资产的归属地，我们可以在 Shopify 中通过点击左下角的 Settings 链接，然后点击 Files 链接来找到它。除了充当我们所有资产的数据库，即 Files 部分之外，我们还可以使用右上角的 Upload files 按钮直接向 Files 部分上传资产。此外，我们可以轻松恢复任何资产的直接 URL 路径并使用它。

在下面的代码块中，我们可以看到如何使用 `image_picker` 输入的示例：

```
{
  "type": "image_picker",
  "id": "banner_image",
  "label": "Banner image"
}
```

请注意，`image_picker` 类型输入值将始终返回一个图像对象值，如果未定义该值则返回 nil：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_221_0.png)

图 6.18 – `image_picker` 专用输入类型的示例

由于 `image_picker` 返回一个图像对象，我们可以使用 `img_tag` 或 `img_url` 过滤器动态生成必要的图像标签：

```
<img src="{{ settings.banner_image | img_url: "600x600" }}" alt="settings.banner_image.alt }}"/>
```

```
{{ settings.banner_image | img_tag: image_item.alt, "class1 class2", "600x600" }}
```

有关 `img_tag` 和 `img_url` 过滤器使用的详细提醒，我们可以重新查看第 5 章《Diving into Liquid Core with Filters》中的 HTML 和 URL 过滤器部分。

## The font_picker input

在学习 `select` 输入类型时，我们提到过 Shopify 为我们提供了访问庞大字体库的权限。`font_picker` 类型输入允许我们创建一个字体选择器选择字段，我们可以使用它在 Shopify 字体库中选择任何字体：

```
{
  "type": "font_picker",
  "id": "body_font",
  "label": "Body font",
  "default": "helvetica_n4"
}
```

与标准属性集相比的一个额外变化是，`default` 属性现在是强制性的。我们可以在以下链接 https://shopify.dev/themes/architecture/settings/fonts#available-fonts 中找到 `default` 属性的可能字体句柄值，通过点击每个字体文件名旁边的 More info 按钮。下面的屏幕截图向我们展示了 `font_picker` 类型输入的示例：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_222_0.png)

图 6.19 – `font_picker` 专用输入类型的示例

我们需要做的第一件事是创建一个变量来保存 `font_picker` 对象值，之后我们需要使用 `font_face` 过滤器调用该变量：

```
<style>
{% assign body_font = settings.body_font %}
{{ body_font | font_face }}
</style>
```

在声明了 `font_face` 之后，我们现在可以轻松地从变量内部访问 `@font-face`。

假设现在我们想要修改特定元素的特定属性，例如 `font-weight` 和 `font-style`。要实现这种类型的功能，我们可以使用 `font_modify` 过滤器，它接受两个属性，即 `style` 属性，允许我们修改 `font-style`，以及 `weight`，我们可以使用它来修改 `font-weight` 属性：

```
<style>
{% assign body_font = settings.body_font %}
{{ body_font | font_face }}

{% assign body_font_bold = body_font | font_modify: "weight", "bolder" %}
{% assign body_font_italic = body_font | font_modify: "style", "italic" %}
{% assign body_font_bold_italic = body_font_bold | font_modify: "style", "italic" %}
{{ body_font_bold | font_face }}
{{ body_font_italic | font_face }}
{{ body_font_bold_italic | font_face }}
</style>
```

请注意，我们现在有三个不同的变量包含三种不同类型的 `@font-face`。剩下的就是提取我们需要的特定属性并将其分配来设置内容样式。

我们可以通过使用字体对象的 `family`、`style` 和 `weight` 属性来返回特定的 `@font-face` 属性值：

```
```liquid
<style>
  {% assign body_font = settings.body_font %}
  {{ body_font | font_face }}

  {% assign body_font_bold = body_font | font_modify: "weight", "bold" %}
  {% assign body_font_italic = body_font | font_modify: "style", "italic" %}
  {% assign body_font_bold_italic = body_font_bold | font_modify: "style", "italic" %}
  {{ body_font_bold | font_face }}
  {{ body_font_italic | font_face }}
  {{ body_font_bold_italic | font_face }}

  .body_bold {
    font-family: "{{ body_font_bold.family }}";
    font-style: "{{ body_font_bold.style }}";
    font-weight: "{{ body_font_bold.weight }}";
  }
  .body_italic {
    font-family: "{{ body_font_italic.family }}";
    font-style: "{{ body_font_italic.style }}";
    font-weight: "{{ body_font_italic.weight }}";
  }
</style>
```
```

我们现在已经成功学习了 `font_picker` 输入类型的工作原理，更重要的是，我们还学习了如何使用字体对象和过滤器输出 `font_picker` 值并使字体选择过程完全动态化。

使我们的代码更具功能性的唯一补充是，如果所选字体族因某种原因无法渲染，则包含回退族。我们可以通过引入 `fallback_families` 对象来做到这一点，它将返回建议的回退字体族：

```
```liquid
<style>
  {% assign body_font = settings.body_font %}
  {{ body_font | font_face }}

  {% assign body_font_bold = body_font | font_modify: "weight", "bolder" %}

  {% assign body_font_italic = body_font | font_modify: "style", "italic" %}

  {% assign body_font_bold_italic = body_font_bold | font_modify: "style", "italic" %}
  {{ body_font_bold | font_face }}
  {{ body_font_italic | font_face }}
  {{ body_font_bold_italic | font_face }}

  .body_bold {
    font-family: "{{ body_font_bold.family }}",
                 "{{ body_font_bold.fallback_families }}";
    font-style: "{{ body_font_bold.style }}";
    font-weight: "{{ body_font_bold.weight }}";
  }
  .body_italic {
    font-family: "{{ body_font_italic.family }}",
                 "{{ body_font_italic.fallback_families }}";
    font-style: "{{ body_font_italic.style }}";
    font-weight: "{{ body_font_italic.weight }}";
  }
</style>
```
```

有关所有可用字体过滤器的更多信息，我们可以参考 https://shopify.dev/api/liquid/filters/font-filters，关于所有可用字体对象的信息，我们可以参考 https://shopify.dev/api/liquid/objects/font。

## The article input

`article` 类型输入为我们提供了特殊的文章选择器选择字段。通过文章选择器，我们可以访问商店中所有可用的文章：

```
{
  "type": "article",
  "id": "featured_article",
  "label": "Featured article"
}
```

直到最近，`article` 输入类型始终会返回文章的字符串句柄，然后需要使用它来恢复文章对象。然而，自 Shopify Unite 2021 大会以来，`article` 和其他页面相关输入现在返回一个对象，使我们的工作变得更加轻松。

请注意，虽然页面相关输入类型将返回一个对象，我们可以从中提取任何所需的值，但我们有时仍会发现自己正在处理一个主题，该主题使用旧方法来检索页面相关输入类型对象。出于这个原因，我们将提及这两种方法，因为虽然我们不会使用旧方法，但如果我们需要处理它，了解其工作原理至关重要。在下面的屏幕截图中，我们可以看到 `article` 输入类型的示例：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_227_0.png)

图 6.20 – `article` 专用输入类型的示例

由于 `article` 输入值返回一个对象值，我们已经完全访问了文章对象，并且可以轻松检索我们可能需要的任何属性：

```
{% assign article = settings.featured_article %}
```

正如我们所见，现在 `article` 输入返回一个对象，访问对象本身并检索任何属性的值几乎毫不费力。现在让我们深入研究使用文章句柄检索文章对象的过时方法。

