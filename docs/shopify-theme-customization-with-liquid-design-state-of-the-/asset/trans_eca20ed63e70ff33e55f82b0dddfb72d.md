

<p>Lorem ipsum content<p>
</div>
<div class="product-single__description-item">
<span>Instructions</span>
<p>Lorem ipsum content<p>
</div>

查看结果后，我们可以看到我们已成功提取每个产品描述块。然而，还有一些问题需要解决。例如，我们会得到一个空的 div，我们可以通过引入 offset 参数并将其值设置为 1 来快速解决，这样就可以跳过 for 循环中的第一次迭代。我们可以通过访问第 3 章《深入 Liquid 核心与标签》中的 Iterations 标签部分来提醒自己 offset 参数：

```
<div class="product-single__description rte">
{% assign productDescription = product.description | split: "<h6>" %}
{% for item in productDescription offset: 1 %}
<div class="product-single__description-item">
{{ item }}
</div>
{% endfor %}
</div>
```

向 for 标签添加 offset 参数会跳过第一次迭代，否则会返回空值。此外，我们已确保我们的数组仅包含最初打算的三个产品描述块。但是，让我们更仔细地查看应用 offset 参数后我们数组的当前结果：

```
<div class="product-single__description-item">
<span>Description</span>
<p>Lorem ipsum content<p>
</div>
<div class="product-single__description-item">
<span>Ingredients</span>
<p>Lorem ipsum content<p>
</div>
<div class="product-single__description-item">
<span>Instructions</span>
<p>Lorem ipsum content<p>
</div>
```

正如我们所回忆的，通过使用 opening h6 标题作为 split 过滤器的子字符串，我们将自动删除初始字符串中的任何 opening h6 标题。然而，请注意，我们不仅删除了 opening h6 标签，还删除了 closing h6 标签。

从技术上讲，closing h6 标签仍然存在，位于 closing span 标签之后。然而，由于我们删除了 opening h6 标签，浏览器将其解释为错误并自动删除了 closing h6 标签。因此，与其依赖浏览器来清理，不如使用 closing h6 标签来进一步划分我们的三个块。

目前，for 循环中的每个 item 变量都包含标题和内容文本。通过应用使用 closing h6 标题的 split 过滤器，我们将修改 item 变量，使其成为自己的数组，包含标题和内容。因此，与其使用另一个 for 循环来遍历它们，不如回忆一下我们之前提到的 first 和 last 过滤器，并在此处使用它们来分别恢复每个值：

```
<div class="product-single__description rte">
{% assign productDescription = product.description | split: "<h6>" %}

{% for item in productDescription offset: 1 %}
<div class="product-single__description--item">
<div class="product-single__description-title">
{{ item | split: "</h6>" | first }}
</div>
<div class="product-single__description-content">
{{ item | split: "</h6>" | last }}
</div>
</div>
{% endfor %}
</div>
```

通过再次应用 split 过滤器，我们已删除剩余的 closing h6 标签。此外，我们更清晰地分离了内容，使我们能够更轻松地使用结果来完成我们的迷你项目。

此时，我们拥有完成产品手风琴项目所需的所有必要元素。唯一剩下的是包含一些样式并引入脚本来处理 on-input 动画。为了保持简洁，我们不会在本项目中介绍样式和功能。然而，我们将提供最终预期以帮助我们可视化一个清晰的目标，我们应该为此努力，因为它将成为未来工作的绝佳练习：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_177_0.png)

图 5.6 – 完整的产品手风琴项目示例

到目前为止，我们学习了如何通过将字符串变量转换为数组类型变量并使用它们来创建产品手风琴功能来操作字符串变量。此外，我们还学习了使用管道字符包含的 `split` 和 `first/last` 过滤器。然而，具体过滤器，例如 `size`，可以根据情况使用管道和点注释与管道一起使用：

```
```liquid
{% assign message = "This is a short string message." %}

{{ message | size }}
{% if message.size >= 10 %}
    This message contains more than 10 characters.
{% endif %}
```
```

在前面的示例中，我们创建了一个 message 变量并为其分配了一个简短的字符串。使用 size 过滤器和 message 变量，我们将呈现 message 字符串中的总字符数。然而，通过使用点注释的 size，我们获得了将 size 过滤器用作 Liquid 逻辑一部分的能力：

```
31
This message contains more than 10 characters.
```

我们还可以使用 size 过滤器改进我们的产品手风琴，通过包含一个 if 语句来检查产品描述是否包含超过 1 个代码块，如果是则呈现产品手风琴。否则，它应仅输出常规产品描述：

```
<div class="product-single__description rte">
{% assign productDescription = product.description | split: "<h6>" %}

{% if productDescription.size > 1 %}
{% for item in productDescription offset: 1 %}
<div class="product-single__description--item">
<div class="product-single__description-title">
{{ item | split: "</h6>" | first }}
</div>
<div class="product-single__description-content">
{{ item | split: "</h6>" | last }}
</div>
</div>
{% endfor %}
{% else %}
<div class="rte">{{ product.description }}</div>
{% endif %}
</div>
```

通过引入 size 过滤器，我们使代码变得更加简洁和优化，因为我们不会为数组类型变量中的单个项目运行 for 循环。

> **提示：**
> 
> 有关所有可用字符串过滤器的更多信息，我们可以参考
> `https://shopify.dev/docs/themes/liquid/reference/filters/string-filters`。
> 
> 有关所有可用数组过滤器的更多信息，我们可以参考
> `https://shopify.dev/docs/themes/liquid/reference/filters/array-filters`。

我们看到了看似无关紧要的过滤器如何成为强大的工具，使我们能够创建商家经常寻求的复杂功能。通过这种类型的学习过程，我们通过在真实项目上工作获得了宝贵的经验，但我们也学会了如何处理各种类型的过滤器，这比简单地列出它们重要得多。

# 数学和货币过滤器

在上一章中，我们在处理 `Custom collections` 项目时有机会看到货币过滤器的实际应用。**货币过滤器**是简单类型的过滤器，其唯一任务是根据货币格式选项格式化数值，*但这到底意味着什么？*

为了更好地理解，让我们导航到我们的管理页面并点击左下角的 **Settings** 按钮。因此，点击 **General** 选项以打开我们将能够更新商店基本信息的位置。进入后，向下滚动直到到达名为 **Store currency** 的部分。这就是我们可以更改商店默认货币的地方，我们的客户将使用它进行购买。与其更改商店货币，不如点击 **Change formatting** 按钮。

![](img/4e10d2f6d65d6d1c551f760d91c2c116_180_0.png)

图 5.7 – 商店货币格式的位置

另一方面，**数学过滤器**不言自明，因为它们允许我们执行特定的数学任务。同样，与字符串和数组过滤器一样，我们可以在一行中链接多个数学过滤器，在这种情况下，过滤器将按从左到右的顺序应用。

现在我们已经熟悉了货币和数学过滤器，让我们看看它们在实际中的效果，并开始我们的下一个迷你项目。

# 产品折扣价

在以下项目中，我们将通过处理我们在第 4 章《深入 Liquid 核心与对象》中开始的项目之一，即位于全局对象部分下的 Custom collection 来学习数学和货币过滤器。

我们的目标是创建产品折扣价并更新销售徽章以显示实际的折扣百分比。让我们开始导航到我们之前开发的代码位置。

我们通过在 Templates 目录下的 collection.liquid 文件中包含 collection-form.liquid snippet 来创建 Custom collection 功能。在 collection-form.liquid snippet 中，我们将看到以下代码：

```
{% if product.compare_at_price != blank %}
<div class="custom-collection--item">
  <a href="{{ product.url }}">
    <img src="{{ product | img_url: "300x300" }}"/>
    <p class="h4 custom-collection--title">{{ product.title }}</p>
    <p class="custom-collection--price">
      {{ product.price | money }}
      <span>{{ product.compare_at_price | money }}</span>
    </p>
    <span class="custom-collection--sale-badge">sale</span>
  </a>
  {% form "product", product %}
    <input type="hidden" name="id" value="{{ product.first_available_variant.id }}" />
    <input type="submit" value="Add to Cart"/>
  {% endform %}
</div>
{% endif %}
```

