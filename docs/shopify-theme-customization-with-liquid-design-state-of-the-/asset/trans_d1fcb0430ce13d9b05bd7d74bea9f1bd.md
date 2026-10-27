

## 构建产品画廊

在这个迷你项目中，我们将学习如何输出产品画廊所需的元素：

- 1. 让我们先创建一个名为 `Product Gallery` 的新页面，以及一个名为 `product-gallery` 的新页面模板，我们将把该模板分配给之前创建的页面。
- 2. 在创建页面并分配适当的模板后，我们应该确定一个拥有多个图片的产品并获取其句柄。我们将使用园艺手套作为我们的选择，它是我们之前从 *第 3 章，使用标签深入 Liquid 核心* 中的 `product-data.csv` 文件导入的产品之一。我们可以通过预览产品页面并从页面 URL 中复制页面句柄来检索产品句柄。或者，我们可以通过导航到管理区内的产品页面并从页面底部的页面 URL 链接中复制页面句柄来检索它。
- 3. 现在我们已经获取了产品句柄，让我们开始通过其句柄创建一个产品对象并将其分配给一个变量。为了实现这一点，我们可以使用之前学到的通过句柄访问页面对象的方法。然而，与之前章节中通过其集合访问产品对象相比，直接使用句柄访问产品对象略有不同。

我们将使用一个名为 `all_products` 的全局对象标签，而不是复数化产品对象名称，它使我们能够访问商店中的所有产品。

> **重要说明：**
> 
> 虽然 `all_products` 是一个相当实用的方法，允许我们直接通过句柄访问任何产品，但它有一个限制，即每个页面只能运行 20 次。这意味着如果我们需要在单个页面上恢复超过 20 个特定产品，我们将需要通过循环遍历集合来恢复它们。

- 4. 我们可以通过使用 `all_products` 全局对象，后跟我们要访问的产品句柄，来通过其句柄访问产品对象。对于我们的示例，我们将使用方括号，`[]`。然而，我们也可以使用点 `.` 注释：

```
```liquid
{% assign product_object = all_products["gardening-gloves"] %}
```
```

这样，我们现在就可以访问园艺手套产品对象了。要访问附加到产品的所有图片，我们将使用 `product_object` 变量作为我们的对象，后跟 `images` 属性，以恢复特定产品的图片数组。由于我们处理的是一个数组，我们将不得不使用 `for` 标签来循环遍历它们：

```
```liquid
{% assign product_object = all_products["gardening-gloves"] %}
{% for image_item in product_object.images %}
  <img src="{{ image_item | img_url }}"/>
{% endfor %}
```
```

我们现在已成功提取了产品图片数组，我们可以使用它结合各种滑块插件创建强大的画廊。然而，请注意，我们在商店前端上的图片尺寸相对较小。

这是因为如果我们不引入 `size` 参数，Liquid 总是默认使用 100x100 的尺寸。让我们通过将图片宽度和高度限制为 300 像素来引入 `size` 参数：

```
```liquid
{% assign product_object = all_products["gardening-gloves"] %}
{% for image_item in product_object.images %}
  <img src="{{ image_item | img_url: "300x300" }}"/>
{% endfor %}
```
```

请注意，我们的图片现在宽度为 300 像素，但高度只有 200 像素，即使我们指定了希望高度为 300 像素。这是因为 `size` 参数只能通过将图片尺寸减小到指定的值来限制图片大小。它不能改变图片的宽高比，也不能将图片尺寸增加到超过原始图片尺寸。

在前面的示例中，我们使用 "300x300" 来限制产品图片的宽度和高度。然而，我们也可以只限制一侧，通过使用 "300x" 将图片宽度设置为 300 像素，或使用 "x300" 将高度限制为 300 像素。如果我们只指定两个值中的一个，Shopify 将在保持图片宽高比的同时自动计算图片的尺寸。

我们可以用 `img_url` 过滤器使用的第二个参数是 crop 参数，它允许我们在结合 `size` 参数时裁剪图片到指定尺寸。crop 参数可以有五个不同的值：

- top
- center
- bottom
- left
- right

使用正确的选项，我们可以指定想要裁剪掉的图片的哪一侧。对于我们的示例，我们可以使用 center 选项，以确保图片从每一侧均匀裁剪：

```
{% assign product_object = all_products["gardening-gloves"] %}
{% for image_item in product_object.images %}
<img src="{{ image_item | img_url: "300x300", crop:
  "center" }}" />
{% endfor %}
```

通过使用 crop 参数更改图片尺寸，我们也改变了图片的宽高比，因为所有图片现在宽度和高度都正好是 300 像素。

我们可以用 `img_url` 过滤器使用的最后两个参数是 scale，它允许我们使用 2 和 3 作为其选项值来指定图片的像素密度，以及 format，这是一个相当有趣的参数，允许我们指定显示图片的格式。format 参数的两个可接受值是 jpg 和 pjpg。

使用 pjpg，我们可以将图片格式转换为渐进式 JPEG，自动加载全尺寸图片并逐渐提高其质量，而不是像传统 JPEG 那样从上到下加载图片（我们可以在以下链接阅读有关渐进式 JPEG 的更多信息：https://en.wikipedia.org/wiki/JPEG#JPEG_compression）：

```
{% assign product_object = all_products["gardening-gloves"] %}
{% for image_item in product_object.images %}
<img src="{{ image_item | img_url: "300x300", crop:
  "center", format: "pjpg" }}" />
{% endfor %}
```

正如我们所见，`image_url` 和 `img_tag` 过滤器都有其有用的参数，而 `img_tag` 更简洁，但它有限制，因为我们只能在生成的 HTML `img` 标签中包含有限数量的属性。

假设我们希望从 `Assets` 目录中提取一个图片文件，并应用适当的 `size` 参数将其用作背景图片。我们不能使用 `img_tag`，如前所述，因为它会返回一个 HTML `img` 标签。我们也不能单独使用 `asset_url`，因为 `asset_url` 不接受任何额外参数，包括 `size` 参数。

与 `stylesheet_tag` 和 `script_tag` 类似，我们可以使用特殊的 `asset_img_url` 过滤器，它允许我们包含 `size` 参数以从 `Assets` 目录中恢复图片：

```
{{ "ajax-loader.gif" | asset_img_url: "300x300", scale: 2, crop: "center" }}
```

请注意，`asset_img_url` 允许我们包含 `size` 参数和其他之前与 `img_url` 过滤器一起可用的参数，包括 `size`、`crop`、`scale` 和 `format`。

到目前为止，我们已经学习了如何访问 `Assets` 目录中的不同类型文件并为每种文件生成适当的 HTML 标签。我们还通过一个小型项目练习了如何使用 `img_url` 过滤器，输出创建基本产品画廊所需的元素。虽然我们没有介绍 Liquid 提供的所有 URL 和 HTML 过滤器，但我们现在已经为使用 HTML 和 URL 过滤器奠定了适当的基础，这些知识在 Liquid 中至关重要，并将对我们未来的工作有很大帮助。

> **提示：**
> 要获取有关所有可用 HTML 过滤器的更多信息，我们可以参考 https://shopify.dev/docs/themes/liquid/reference/filters/html-filters，并要获取有关可用 URL 过滤器的更多信息，我们可以参考 https://shopify.dev/docs/themes/liquid/reference/filters/url-filters。

