

在后台中，每个产品都包含两个不同的价格字段，位于 **定价** 部分下方。第一个字段名为 **价格**，用于设置产品的当前价格。而第二个字段名为 **原价**，用于设置初始价格以模拟折扣。我们可以将 `product` 对象与 `compare_at_price` 属性结合来访问该字段的数据。

让我们回到集合页面上的代码，并修改它，使其同时包含促销徽章和原价，以匹配最初的集合页面。我们可以将 `{{ product.compare_at_price | money }}` 包裹在 span 元素中，并紧跟在 `{{ product.price | money }}` 之后，从而显示原价。此外，我们可以通过创建一个简单的字符串消息，并将其放在价格元素下方的 span 标签内，从而实现促销徽章：

```
{% if product.price < 10000 %}
  <a href="{{ product.url }}">
    <img src="{{ product | img_url }}"/>
    <p>{{ product.title }}</p>
    <p>{{ product.price | money }}<span>{{ product.compare_at_price | money }}</span></p>
    <span>sale</span>
  </a>
  {% form "product", product %}
    <input type="hidden" name="id" value="{{ product.first_available_variant.id }}" />
    <input type="submit" value="Add to Cart"/>
  {% endform %}
{% endif %}
```

我们已经为自定义集合商品包含了所有必要的元素。然而，相当多的产品没有显示其原价和促销徽章，这是因为产品后台中的 **原价** 字段为空。我们可以通过修改第一行的 if 语句来解决第二个问题，使其显示已定义原价的产品：

```
{% if product.compare_at_price != blank %}
  <a href="{{ product.url }}">
    <img src="{{ product | img_url }}"/>
    <p>{{ product.title }}</p>
    <p>{{ product.price | money }}<span>{{ product.compare_at_price | money }}</span></p>
```

到目前为止，我们只提到了属性返回单个值的对象，例如 `product.price`、`product.title` 或 `product.first_available_variant.id`。然而，它们也可以返回数组，甚至可以作为我们编程逻辑的辅助工具。

最常用的返回数组的对象之一是 `linklist` 对象。它与 `link` 对象结合，可以帮助我们从导航后台读取菜单数据，并帮助我们创建自定义导航菜单。

## 自定义导航

在我们的下一个项目中，我们将专门为集合页面创建一个多级导航菜单。然而，在学习 `linklist` 和 `link` 对象之前，我们需要先创建一个导航菜单。让我们开始吧：

1. 我们可以通过进入后台，点击 **在线商店** 展开它，然后点击 **导航** 链接来创建新的导航菜单，在那里我们可以通过点击 **添加菜单** 按钮来创建新菜单：

   ![](img/4e10d2f6d65d6d1c551f760d91c2c116_121_0.png)

   图 4.3 – 访问导航菜单的示例

2. 点击 **添加菜单** 按钮后，将菜单标题设置为 **室内导航**。设置菜单名称后，菜单的句柄将自动填充，因此无需手动修改。

3. 现在我们可以填充菜单了。点击“添加菜单项”按钮，然后点击“链接”字段，它会自动显示一个下拉菜单。对于第一个菜单项，点击“集合”，然后点击“所有集合”，这应该会立即填充菜单项的名称和链接字段。点击“添加”按钮完成该菜单项的添加。

4. 重复上一步六次：创建两个菜单项，分别命名为 **室内** 和 **室外** 并链接到这两个集合，然后创建四个菜单项，分别链接到商店中的任意四个产品。

5. 创建完额外的菜单项后，将 **室内** 和 **室外** 菜单项移动到 **所有集合** 菜单项下，以创建嵌套菜单。我们可以通过点击菜单项名称前的六个点，然后按住并将它们拖到 **所有集合** 菜单项上方，直到出现缩进的蓝色线条，此时松开鼠标即可：

   ![](img/4e10d2f6d65d6d1c551f760d91c2c116_122_0.png)

   图 4.4 – 在导航菜单中创建下拉菜单的示例

6. 重复上一步，将两个产品菜单项移动到 **室内** 集合菜单项下（该菜单项已移动到 **所有集合** 菜单项下）。对 **室外** 集合菜单项和剩余的两个产品菜单项重复相同的过程。

如果一切操作正确，我们最终会得到一个 **室内导航** 菜单，其中包含一个菜单项 **所有集合**，而该菜单项下包含两个集合菜单项，每个集合菜单项下又包含两个产品菜单项。假设按照上述步骤操作后菜单格式看起来不一样，那么我们可以查阅 Shopify 关于嵌套菜单项的文档，在那里可以找到更详细的说明和一个关于此主题的 YouTube 链接。

有关创建嵌套导航的更多信息，请参阅 `https://help.shopify.com/en/manual/online-store/menus-and-links/drop-down-menus`。

> **重要说明：**
> 
> 单个菜单中嵌套菜单项的最大数量最多为三级。我们可以将之前创建的菜单视为三级导航菜单，这已经达到了上限。

现在我们已经成功创建了导航菜单，可以开始创建一个变量，将 `linklist` 对象的值赋给它，并在后面附上我们要访问的导航的句柄。请记住，使用句柄访问对象时，我们需要在对象末尾添加字母 s，使其变为复数形式：

```
{% assign collection-menu = linklists.indoor-navigation %}
```

这样，我们就把室内导航的对象成功保存到了 `collection-menu` 变量中。让我们在 `collection.template` 中测试一下。为此，我们将在 `{% section 'collection-template' %}` 上方调用 `collection-menu` 对象，并在其后访问 title 和 levels 属性，以便我们能看到导航的名称及其拥有的嵌套菜单数量：

```
{% assign collection-menu = linklists.indoor-navigation %}
{{ collection-menu.title }} - {{ collection-menu.levels }}
```

如果我们预览我们的室内集合，我们将能够看到导航的名称以及导航菜单具有的嵌套层级数量：

```
Indoor Navigation - 3
```

```
{% endfor %}
{% endfor %}
{% endfor %}
```

我们现在已经拥有创建一个可用的悬停导航菜单所需的全部元素。剩下唯一要做的事情就是添加一些 HTML 标签，并应用必要的样式来创建悬停下拉效果。然而，在我们继续处理样式之前，让我们尝试更有创意一些。

请注意，我们的菜单链接就只是链接。但如果我们想更有创意一些，在每个产品菜单项旁边显示一张图片呢？要实现这一点，我们需要识别哪些菜单项指向产品，我们可以使用 `type` 属性，并比较返回值是否等于 `product_link` 字符串来实现。由于我们已经知道只有导航的第三级包含产品菜单项，因此我们只会把这个功能放在第三个 for 循环内：

```
{% assign collection-menu = linklists.indoor-navigation %}
{% for link in collection-menu.links %}
{{ link.title }}
{% for sub_link in link.links %}
{{ sub_link.title }}
{% for sub_sub_link in sub_link.links %}
{% if sub_sub_link.type == "product_link" %}
<img src="{{ sub_sub_link | img_url: "250x250" }}"/>
{% endif %}
{{ sub_sub_link.title }}
{% endfor %}
{% endfor %}
{% endfor %}
```

现在，按理说这应该能生效，因此我们应该能够在导航菜单中看到四个产品图片。然而，请注意，sub_sub_link 仍然只是一个 link 对象，而 link 对象并没有附带图片。要显示产品所附带的图片，我们必须取回该链接所指向的产品对象。

