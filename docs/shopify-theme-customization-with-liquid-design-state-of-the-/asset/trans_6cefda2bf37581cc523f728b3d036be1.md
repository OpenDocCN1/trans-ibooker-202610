

# 反向参数

我们列表中的最后一个参数是 `reversed`，顾名思义，它允许我们反转迭代的顺序。reversed 参数没有值表示，我们可以在打开的 `for` 循环标签末尾包含它：

```
```jinja
{% for item in (3..5) reversed %}
    {{ item }}
{% endfor %}
```
```

在前面的示例中，我们将 reversed 参数作为次要参数添加到我们的 `range` 参数旁边，从而使我们的标签以反向顺序执行迭代：

```
```text
5 4 3
```
```

正如我们自己所见，参数是 `for` 标签的强大补充，我们可以用它来实现所需的迭代类型并获得我们需要的结果。

## cycle 标签

`cycle` 是另一个强大的标签，我们只能将其与 `for` 标签结合使用，以循环遍历一组字符串并按最初定义的具体顺序将其输出到每次迭代。我们可以通过打开带百分号符号的大括号分隔符，后跟单词 `cycle`，后跟任意数量用逗号分隔的字符串来定义 `cycle` 标签。最后，我们用关闭大括号分隔符和百分号符号关闭 `cycle` 标签：

```
```jinja
{% for product in collection.products limit: 4 %}
    <div class="product-item {% cycle "first", "", "", "last" %}"></div>
{% endfor %}
```
```

在前面的示例中，我们在限制为四次迭代的 `for` 循环内包含了一个包含四个不同字符串的 `cycle` 标签。通过实现 `cycle` 标签，我们确保我们的第一个 `div` 元素将接收类 `first`，第四个元素将接收 `last` 类，而中间的两个元素将不会接收任何类，因为这些位置的字符串为空：

```
```html
<div class="product-item first"></div>
<div class="product-item"></div>
<div class="product-item"></div>
```

```
<div class="product-item last"></div>
```

从我们的结果可以看出，cycle 标签是一个非常有价值的工具，我们可以用它以我们定义的有序方式将数据传递给循环中的特定迭代。但是，如果我们从循环中删除限制或将其增加到九次迭代会怎样？

```
{% for product in collection.products limit: 9 %}
    <div class="product-item {% cycle "first", "", "", "last" %}"></div>
{% endfor %}
```

在我们之前的示例中，迭代次数与 cycle 标签内的字符串数量完全相同。在我们最新的示例中，迭代次数高于字符串数量，这意味着 cycle 将重置并开始再次按顺序应用字符串，以满足所需的迭代次数：

```
<div class="product-item first"></div>
<div class="product-item"></div>
<div class="product-item"></div>
<div class="product-item last"></div>
<div class="product-item first"></div>
<div class="product-item"></div>
<div class="product-item"></div>
<div class="product-item last"></div>
<div class="product-item first"></div>
```

到目前为止，我们已经学习了 cycle 标签的基本用法，并且标签将继续为尽可能多的迭代输出字符串，如果我们只有一个 cycle 标签，这非常有帮助，但如果我们有两个或更多个怎么办？

```
{% for product in collection.products limit: 6 %}
    <div class="product-item {% cycle "first", "", "", "last" %}"></div>
{% endfor %}
```

```
{% for product in collection.products limit: 4 %}
    <div class="product-item {% cycle "first", "", "", "last" %}"></div>
{% endfor %}
```

在前面的示例中，我们创建了两个单独的 for 循环，一个限制为六次迭代，另一个限制为四次。正如我们刚刚学到的，无论我们在 cycle 标签内定义了多少个字符串，标签都将继续为所需的迭代次数输出字符串：

```
<div class="product-item first"></div>
```

```
<div class="product-item"></div>
```

```
<div class="product-item"></div>
```

```
<div class="product-item last"></div>
```

```
<div class="product-item first"></div>
```

```
<div class="product-item"></div>
```

```
<div class="product-item"></div>
```

```
<div class="product-item last"></div>
```

```
<div class="product-item first"></div>
```

```
<div class="product-item"></div>
```

正如我们从结果中看到的那样，第一个 for 循环按我们预期的确切顺序创建了六次迭代。然而，第二个 for 循环没有产生相同的结果。相反，它继续从上一个 cycle 标签停止的确切位置开始输出字符串。

考虑到 Liquid 目前无法区分不同类型的 cycle 标签，这种行为是合乎逻辑的。但是，我们可以通过引入一个名为 cycle group 的参数来轻松解决这个问题。

cycle group 参数允许我们通过确保每个 cycle 都从位置一开始输出字符串来分隔 cycle 标签，无论我们是否已在同一页面上使用过 cycle 标签。在 cycle 关键字之后，我们可以通过添加用括号括起来的字符串名称，后跟冒号来包含 cycle group 参数：

```
{% for product in collection.products limit: 6 %}
```

```
  <div class="product-item {% cycle "group1": "first", "", "", "last" %}"></div>
```

```
{% endfor %}
```

```
{% for product in collection.products limit: 4 %}
```

```
  <div class="product-item {% cycle "group2": "first", "", "", "last" %}"></div>
```

```
{% endfor %}
```

- `render`
- `raw`
- `comment`

## layout 标签

正如我们从第一章回忆的那样，在讨论 `Layout` 目录时，我们提到了 `theme.liquid` 文件的重要性，因为我们将在此文件中呈现所有文件和模板。我们就是在此文件中安排页面整体布局的。

我们的页面目前由三个关键元素组成：**header**、**main content** 和 **footer**。假设我们想从产品页面中删除页眉和页脚部分，或者至少用另一组页眉和页脚替换它们。*我们该怎么做？*

1. 实现此目的的第一步是创建一个备用布局文件，我们的主题产品页面将改用它。我们可以通过在代码编辑器中展开 **Layout** 目录并单击 **Add a new layout** 按钮来完成此操作，这将触发一个弹出窗口以选择我们希望创建的布局页面类型并选择我们新页面的名称。
2. 在下拉菜单中，我们将选择 **theme** 选项作为布局类型，并在以下字段中输入 `alternate` 作为文件名。一旦我们完成选择布局文件类型和名称，请单击 **Create layout** 按钮以完成该过程：

![](img/4e10d2f6d65d6d1c551f760d91c2c116_98_0.png)

图 3.10 – 创建新布局文件的示例

这样，我们就成功创建了一个名为 `theme.alternate.liquid` 的新布局文件。假设我们尝试将此文件与原始 `theme.liquid` 进行比较；我们会发现它们完全相同。原因是在我们创建新页面、布局或模板时，Shopify 不会完全创建新页面，而是会复制主题已在使用的默认页面。例如，如果我们要展开 **Templates** 目录并创建新的页面模板，新页面模板将包含我们之前对 `page.liquid` 文件所做的所有更改。

现在我们已经创建了新的布局文件，我们将将其分配给我们的产品页面。我们可以通过展开 **Templates** 目录并单击 `product.liquid` 文件来完成此操作，在该文件中我们可以为此特定模板定义布局，我们应该将其包含在文件的第一行。我们可以用一组带百分号符号的大括号分隔符围绕单词 `layout`，后跟布局文件名，我们应该将其括在括号内来定义布局标签：

```
```liquid
{% layout "theme.alternate" %}
```
```

如果我们现在导航到 **theme.alternate** 模板并删除 `{% section 'header' %}` 和 `{% section 'footer' %}`，页眉和页脚部分将不再在我们的产品页面上可见。但是，它们仍将在使用原始 `theme.liquid` 布局的页面上可见。

> **重要提示：**
> 
> 除了布局文件名外，layout 标签还接受 `none` 关键字作为其值，且不带括号，在这种情况下，页面将不使用任何布局文件。在这种情况下，使用此特定模板的特定页面将无法访问通过 `theme.liquid` 文件最初加载的任何代码或文件，包括 `.css`、`.js` 或其他文件。

任何不包含 layout 标签的模板文件默认将使用原始 `theme.liquid` 布局作为后备。

## liquid 和 echo 标签

`liquid` 标签是 Liquid 的最新添加之一，它非常强大，因为它允许我们在一个分隔符集中编写多个标签，使我们的代码更易于阅读和维护。我们可以用左边的花括号分隔符和百分号符号定义 liquid 标签，后跟单词 liquid，然后是所需的任意多行 Liquid 代码。请注意，我们只需要定义一个打开的 `liquid` 标签，其中 `liquid` 关闭标签会自动关闭。

