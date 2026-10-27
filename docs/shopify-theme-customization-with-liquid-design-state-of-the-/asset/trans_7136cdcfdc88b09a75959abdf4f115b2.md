

将视频作为产品媒体资源上传的方式与上传图片相同。要上传视频，请在管理侧边栏中点击 **Products** 部分并选择要为其上传视频资源的商品。进入后，向下滚动到 **Media** 部分并点击 **Add Media** 按钮开始上传过程。

> **重要提示：**
>
> 除了外部视频外，将资源上传到 Shopify 平台对所有资源，包括图片和视频，都有特定的限制。除了我们可以使用特定类型的格式外，图片文件不能超过 20 MP 的分辨率，大小不能超过 20 MB，且必须为 `.jpeg` 或 `.png` 格式，而视频文件限制为最长 60 秒，大小不能超过 20 MB，且必须为 `.mp4` 或 `.mov` 视频格式。

上传视频后，我们也可以通过与外部媒体相同的方式添加可选信息，包括 alt 文本和海报图片。

要输出上传到产品媒体文件中的视频，我们需要使用 `video_tag`，可通过 video 对象访问：

```liquid
{% assign product_object = all_products["gardening-gloves"] %}
{% for media in product_object.media %}
  {% case media.media_type %}
    {% when "image" %}
      {{ media | img_tag: media.alt, "class1 class2", "300x300" }}
    {% when "external_video" %}
      {% if media.host == "youtube" %}
        {{ media | external_video_url: controls: 0, color: "white" | external_video_tag }}
      {% else %}
        {{ media | external_video_url: loop: 1, muted: 1 | external_video_tag }}
      {% endif %}
    {% when "video" %}
      {{ media | video_tag }}
    {% when "model" %}
    {% else %}
  {% endcase %}
{% endfor %}
```

通过预览 **Product Gallery** 页面，我们会注意到视频所在的位置只有一张静态图片，而且是一张相对较小的静态图片。如果我们检查收到的结果，会注意到我们已正确生成了 HTML `video` 标签。然而，由于我们没有启用控件，它们默认被隐藏了。此外，如果我们仔细观察，会注意到 HTML `video` 标签中 `poster` 属性的 URL 尺寸很小，这就是商店前端图片很小的原因。

正如我们在学习图片滤镜时所回忆的，如果我们尝试输出图片而不定义图片尺寸，Shopify 会自动将图片调整为 100x100。

使用 `video_tag`，我们可以包含与常规 HTML `video` 标签一起使用的任意数量的属性，包括 `image_size` 参数，从而修改视频 `poster` 属性的尺寸。让我们修改 `video_tag` 以使控件可见，设置海报图片尺寸，并将视频尺寸设置为 300x 以匹配海报尺寸：

```liquid
{% assign product_object = all_products["gardening-gloves"] %}
{% for media in product_object.media %}
  {% case media.media_type %}
    {% when "image" %}
      {{ media | img_tag: media.alt, "class1 class2", "300x300" }}
    {% when "external_video" %}
      {% if media.host == "youtube" %}
        {{ media | external_video_url: controls: 0, color: "white" | external_video_tag: class: "youtube_video" }}
      {% else %}
        {{ media | external_video_url: loop: 1, muted: 1 | external_video_tag: class: "vimeo_video" }}
      {% endif %}
    {% when "video" %}
      {{ media | video_tag: controls: true, image_size: "300x300", width: "300x" }}
    {% when "model" %}
    {% else %}
  {% endcase %}
{% endfor %}
```

有了 `model_viewer_tag`，我们已涵盖了全部四种媒体类型，并确保我们将每种媒体类型以适当的媒体标签表示。然而，请注意我们仍然有一个最终的 switch 语句没有任何媒体标签。

我们可以将最终的 switch 语句视为一种故障保护，如果出于某种原因，之前的任何 switch 语句或媒体未能产生结果，我们将使用它配合 `media_tag`。

`media_tag` 滤镜有点特殊，因为该滤镜会自动为前面提到的四种媒体类型中的任何一种生成适当的媒体标签。因此，如果我们可以使用 `media_tag` 自动生成所有媒体标签，为什么不使用 `media_tag` 生成所有媒体文件呢？为什么我们还要费心学习到现在为止的所有媒体标签？

虽然 `media_tag` 确实会自动检测每种媒体类型并为其渲染适当的媒体标签，但使用 `media_tag` 会失去为特定媒体类型分配自定义类名和属性的能力。因此，我们绝不应将 `media_tag` 用作渲染媒体资源的主要选项。相反，我们应将其用作故障保护，以确保在商店前端正确渲染媒体资源：

```liquid
{% assign product_object = all_products["gardening-gloves"] %}
{% for media in product_object.media %}
  {% case media.media_type %}
    {% when "image" %}
      {{ media | img_tag: media.alt, "class1 class2", "300x300" }}
    {% when "external_video" %}
      {% if media.host == "youtube" %}
        {{ media | external_video_url: controls: 0, color: "white" | external_video_tag: class: "youtube_video" }}
      {% else %}
        {{ media | external_video_url: loop: 1, muted: 1 | external_video_tag: class: "vimeo_video" }}
      {% endif %}
    {% when "video" %}
      {{ media | video_tag: controls: true, image_size: "300x300", width: "300x" }}
    {% when "model" %}
      {{ media | model_viewer_tag }}
    {% else %}
      {{ media | media_tag }}
  {% endcase %}
{% endfor %}
```

到目前为止，我们已学习如何输出图片类型媒体文件，在不同类型的外部媒体资源之间进行区分，为托管在 Shopify 平台上的视频渲染媒体标签，以及为 3D 模型生成适当的媒体标签。最后，通过 `media_tag`，我们已涵盖了 Shopify 目前支持的每种媒体类型，并确保我们将在商店前端正确展示每种媒体资源。

现在我们已拥有创建媒体画廊所需的所有资源，唯一剩下要做的就是通过包含一些 HTML 元素来完善代码，以便正确格式化。除了代码格式外，我们还可以根据想要创建的画廊使用一些滑块插件，例如 Slick。为了保持简洁，我们将不涵盖媒体画廊的样式和功能。然而，对于希望通过完成项目来测试技能的人，可以在以下 Shopify 文章中找到有关样式和功能的必要建议（https://shopify.dev/tutorials/add-theme-support-for-rich-media-3d-and-video）。

在文章中，我们可以找到诸如使用宽高比框来创建响应式布局的信息，或有关功能的一些常见问题的答案，例如将缩略图连接到主画廊或变体本身。

> **提示：**
>
> 有关所有可用媒体滤镜的更多信息，我们可以参考 https://shopify.dev/docs/themes/liquid/reference/filters/media-filters。

在完成本项目时，我们创建了产品媒体画廊的基础版本，使我们能够将任何类型的产品媒体输出到任何页面。虽然它可能看起来并不令人印象深刻，但本项目所学到的知识教会了我们如何创建当今最受欢迎的功能之一，并且在我们的知识不断增长时，将极大地帮助我们创建更高级的功能。

## Building product accordions

在以下项目中，我们将通过处理和创建产品手风琴功能来学习字符串和数组滤镜。*但是，在继续项目之前，字符串和数组滤镜到底做什么？*

**String filters** 是允许我们操作 Liquid 代码输出或变量本身的方法，只要变量是字符串类型，而数组滤镜则允许我们操作数组的输出。

对于本项目，我们将首先找到一个描述较长的商品。为了节省时间，我们已在从 *Chapter 3, Diving into Liquid Core with Tags* 中 `product-data.csv` 文件导入的 **Black Armchairs** 商品中包含了必要的描述。

![](img/4e10d2f6d65d6d1c551f760d91c2c116_169_0.png)

Figure 5.4 – Example of a long product description

