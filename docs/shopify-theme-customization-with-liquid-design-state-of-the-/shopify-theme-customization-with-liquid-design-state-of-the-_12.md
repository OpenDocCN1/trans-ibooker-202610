# Assessments

## Chapter 1, Getting Started with Shopify

### Question 1
Partners program 是什么？

### Answer
Partners program 是 Shopify 创建的一个平台，汇集了来自世界各地的人。通过该平台，我们可以为商店所有者构建新的电商商店，设计主题，开发应用，向 Shopify 推荐新客户，最重要的是，为我们自己创建一个开发商店用于练习。我们可以在 *How to start?* 主题中提醒自己这一点。

### Question 2
我们如何禁用开发类型商店的密码保护？

### Answer
我们可以通过在 **Themes** 部分的横幅上点击 **See store password** 按钮，该部分位于 Online store 部分下，或选择 **Online store** 然后点击展开的下拉菜单中的 **Preferences** 来禁用我们的密码保护商店。进入后，我们可以轻松移除常规商店的密码保护。然而，由于我们的商店处于开发模式，此选项当前处于禁用状态。我们可以在 *Understanding theme structure* 主题中的 *Sidebar* 子主题中提醒自己这一点。

### Question 3
Layout 和 Templates 目录文件之间有什么区别？

### Answer
**Layout** 目录是我们主题的主要目录。它包含基本文件，并且所有其他文件，包括模板文件，都将在此渲染。模板文件是一组文件，允许我们一次性轻松创建和管理多个页面的外观。我们可以在 *Understanding theme structure* 主题中的 *Sidebar* 子主题中提醒自己这一点。

### Question 4
在什么情况下新的模板文件会在页面的管理区内可见？

### Answer
考虑到 Shopify 的管理端只能从当前已发布的主题读取值，我们必须满足两个条件。除了创建新的模板文件外，我们还需要确保同一模板文件存在于当前在线主题中，或将我们的重复主题发布上线。我们可以在 *Understanding theme structure* 主题中的 *Templates* 子主题中提醒自己这一点。

### Question 5
什么类型的文件以及在什么条件下允许我们访问父文件作用域内的变量？

### Answer
**Snippets** 文件允许我们通过引用其名称在 **Templates/Sections** 中重用重复的代码片段。除了允许我们重用代码部分外，**Snippets** 还允许我们在将这些变量作为参数传递给 snippet 的前提下，访问父元素内的变量。

## Chapter 2, The Basic Flow of Liquid

### Question 1
如果我们期望得到一个输出结果，应该使用什么类型的分隔符？

### Answer
如果我们期望从 Liquid 代码得到输出，我们应该使用双括号分隔符，因为我们只应在执行某种逻辑时使用带百分号的括号。我们可以在 *Understanding Liquid and its delimiters* 部分中了解这一点。

### Question 2
以下条件的执行结果是什么，为什么？

```
{% if collection.all_products_count > "20" %}
    The number of products in a collection is greater
      than 20!
{% endif %}
```

### Answer
考虑到 `collection.all_products_count` 默认返回一个数值，而我们与之比较的值是一个字符串，因为它被括在引号内。由于我们无法比较不同类型的值，该条件将返回 false，我们的消息将不会显示。我们可以在 *Learning the comparison operators* 部分中了解这一点。

### Question 3
访问数组中一项的两种方法是什么？

### Answer
我们可以使用两种方法访问数组中的项。第一种方法允许我们使用项的索引位置来恢复我们正在查找的确切项，而第二种方法允许我们遍历数组中的所有项。我们可以在 *Understand the type of data* 部分中的 *Array* 子部分中了解这一点。

### Answer
如果我们希望创建一个循环，跳过前三次迭代并最多输出七次迭代，我们应该使用 `offset` 和 `limit` 参数的组合。`offset` 标签允许我们跳过任意数量的迭代，具体取决于我们分配给它的值。`limit` 参数允许我们限制标签应执行的迭代次数。我们可以通过重新访问 *Iterations tags* 部分的 *for parameters* 子部分来提醒自己这一点。

### Question 2
使用 `capture` 标签创建的变量可以分配什么类型的数据？

### Answer
我们可以使用 `capture` 标签创建的变量分配任何类型的数据。然而，使用 `capture` 标签创建的变量将始终以字符串数据作为结果返回。我们可以通过重新访问 *Variable tags* 部分的 *capture* 子部分来提醒自己这一点。

### Question 3
以下代码块中的两个问题是什么？

```
liquid for product in collections["outdoor"].products
  if product.price > 10000
    continue
  else
    product.title
  endif
 endfor
```

### Answer
虽然使用 `liquid` 标签允许我们在代码块内消除大括号分隔符，但我们不应将其从 `liquid` 标签中移除。我们应在 `liquid` 标签左侧放置一个带百分号符号的开括号分隔符。我们代码的第二个问题是我们在 `product.title` 前缺少 `echo` 标签，该标签替换了双花括号分隔符。我们可以通过重新访问 *Theme tags* 部分中的 *The liquid and echo tags* 子部分来提醒自己这一点。

### Question 4
我们应采取什么方法通过将现有 class 属性替换为字符串和变量的组合来修改 HTML 生成的产品表单？

### Answer
考虑到 `form` 标签不接受字符串和变量组合作为其参数，我们应首先使用 `capture` 标签将这些值分配给一个变量，然后将其传递给 `form` 标签。我们可以通过重新访问 *Theme tags* 部分下的 *form* 子部分来提醒自己这一点。

### Question 5
如果我们想从父元素传递一个对象，应该使用什么参数？

### Answer
唯一允许我们从父元素向下传递对象的标签是 `render` 标签，即使如此，我们也只能通过使用 `with` 和 `as` 参数来做到这一点。我们应将 `with` 参数的值设置为我们希望传递的对象，而 `as` 参数的值应为我们在 snippet 文件中将使用的变量名。我们可以通过重新访问 *Theme tags* 部分中的 *render* 子部分来提醒自己这一点。

## Chapter 4, Diving into Liquid Core with Objects

### Question 1
在以下代码块中我们缺少什么才能使表单正常工作？

```
{% form "product", product %}
<input type="hidden" value="{{ 
    product.first_available_variant.id }}" />
<input type="submit" value="Add to Cart"/>
{% endform %}
```

### Answer
虽然我们已引入 `id` 变体，这是创建可工作的产品表单所必需的，但我们没有使用名称属性并将其值设为 `id`。我们可以通过返回 *Working with global objects* 部分中的 *Custom collection* 子主题来了解其工作原理。

### Question 2
我们如何通过管理导航中定义的链接访问 product 对象？

### Answer
要通过导航菜单访问 `product` 对象，我们需要使用 `for` 标签遍历导航菜单。一旦我们找到哪个菜单项是 `product_type`，我们就可以使用该菜单项，后跟 `object` 属性，以访问该特定 `product` 对象。我们可以通过返回 *Working with global objects* 部分中的 *Custom navigation* 子主题来了解其工作原理。

### Question 3
访问单个和多个 `metafield` 对象的两种方法是什么？

### Answer
我们可以通过我们希望恢复 `metafield` 的页面的对象，后跟 `metafields` 对象，后跟命名空间，最后后跟键来访问单个 `metafield` 对象。如果我们希望恢复多个 `metafields` 对象，我们将需要使用 `for` 标签遍历其命名空间下的所有 metafields。我们可以通过返回 *Improving the workflow with metafields* 部分来了解其工作原理。




### 问题4

如果我们要捕获`line_item`值并将其在结账页面上隐藏，我们需要对输入元素进行什么调整？

```html
<input type="text" name="properties[Your Name]"
placeholder="Your Name"/>
```

**答案**

如果我们想要捕获`line_item`值，我们需要在方括号内将下划线作为第一个字符引入。通过在`line_item`输入中引入这个下划线，我们将自动从结账页面隐藏特定的`line_item`。但是，这不会在购物车页面上隐藏它。购物车页面将需要一些手动调整。我们可以回到*使用全局对象*部分中的*自定义导航*子主题，来回顾这是如何工作的。

## 第5章，深入Liquid核心与过滤器

### 问题1

假设我们有一个名为`product_handles`的数组，其中包含30个产品的句柄。以下代码中的什么问题会阻止我们成功输出所有30个产品的图像？

```liquid
{% for handle in product_handles %}
  {% assign product_object = all_products[handle] %}
  {% for image_item in product_object.images %}
    <img src="{{ image_item | img_url }}"/>
  {% endfor %}
{% endfor %}
```

**答案**

由于我们要输出超过20个产品，在本例中是30个产品，我们不能使用`all_products`对象，因为`all_products`对象有一个限制：我们在单个页面上只能调用它20次。如果我们想从超过20个产品中获取数据，我们需要将它们分配到一个集合中，然后对这些产品执行循环。我们可以通过访问*使用HTML和URL过滤器*部分来回顾`all_products`对象。

### 问题4

我们可以使用什么过滤器来快速更新主题文件中出现的任何字符串值？

**答案**

为了轻松更新任何出现的字符串值，我们需要使用t（翻译）过滤器。通过定义翻译键，我们可以快速更新甚至翻译任何字符串值，而无需手动更新多个文件中硬编码的字符串值。

## 第6章，配置主题设置

### 问题1

输入设置有哪两种类型？

**答案**

第一组设置称为基础输入类型，它由六种设置类型组成，允许我们输出基本的HTML输入元素，通过这些元素我们可以动态输出特定内容。第二组设置，也称为专用设置，允许我们生成专用的选择器类型字段，以通过商店访问各种对象，并使用它们的属性输出其内容。我们可以通过访问第6章“配置主题设置”中的“基础输入类型”和“专用输入设置”部分，来回顾这些设置的基础输入类型和专用输入类型。

### 问题2

导致以下代码出错的问题是什么？

```json
{
  "type": "text",
  "id": "header_announcement",
  "label": "Text",
}
```

**答案**

虽然代码结构是正确的，但我们在文本设置类型的最后一个属性后意外地包含了一个额外的逗号，这将导致JSON错误，并阻止我们保存更改。我们可以通过访问第6章“配置主题设置”中的“基本JSON设置”部分来回顾严格的JSON格式。

### 问题3

我们如何在Shopify中包含自定义字体文件并在整个主题编辑器中使用它？

**答案**

虽然font_picker允许我们访问Shopify中的大量字体，但我们无法在此库中包含自定义字体。要包含一组自定义字体，我们必须使用select输入类型的设置，在其中我们可以手动创建我们希望包含的字体列表。我们可以通过访问第6章“配置主题设置”中“基础输入类型”部分的“select输入”子部分来回顾如何包含自定义字体。

### 问题4

哪两个问题会阻止我们执行以下代码？

```json
{
  "type": "range",
  "id": "number_of_products",
  "min": 110,
  "max": 220,
  "step": 1,
  "unit": "pro",
  "label": "Number of products",
  "default": 235
}
```

**答案**

考虑到每个`range`滑块最多可以有100步，第一个问题是`min`和`max`属性值相差太远，我们可以通过减小这两个值之一来解决，使它们之间的步数不超过100。另一种解决方案是将`step`增加到更高的值，从而减少`min`和`max`值之间的步数。

第二个问题是`default`属性值当前超过了`max`属性值。我们可以通过减小`default`值或增大`max`属性值来解决。在确保`default`属性值不超过`max`值之后，我们还需要确保`default`值也高于`min`属性值。我们可以通过访问*第6章，配置主题设置*中*基础输入类型*部分的*范围输入*子部分来回顾`range`类型设置的格式。

## 第7章，使用静态和动态分区

### 问题1

静态和动态分区之间的主要区别是什么？

**答案**

静态和动态分区之间的主要区别在于，我们只能使用主题编辑器中的**添加分区**按钮将动态分区添加到JSON模板和主页。此外，我们可以使用不同的内容任意重复此操作。

另一方面，静态分区需要使用`section`标签手动包含在主题模板中。我们可以在多个模板中包含相同的静态分区。但是，每个分区将显示相同的内容，因为我们只能有一个静态分区的实例。我们可以通过访问*静态与动态分区*部分来回顾静态和动态分区。

### 问题2

我们可以使用什么对象来访问块输入值？编写一些代码，让我们能够访问特定块的模块输入值。

```liquid
{% for block in section.blocks %}
  {% case block.type %}
    {% when "block-type" %}
      {{ block.settings.input-id }}
  {% endcase %}
{% endfor %}
```

**答案**

我们可以通过分区对象访问块的输入值，并将其与blocks属性结合，这将返回一个块对象数组：

我们可以通过访问“使用块构建”部分来回顾如何访问块输入类型。

### 问题3

limit和max_blocks属性之间有什么区别？

**答案**

limit和max_blocks属性之间的主要区别在于，limit属性只允许我们限制特定块类型可以重复的次数。另一方面，max_blocks属性允许我们限制特定分区中可以包含的块数量，无论块类型如何。我们可以通过访问“使用块构建”和“max_blocks属性”部分来回顾如何使用limit和max_blocks属性以及两者之间的区别。

### 问题4

我们如何应用特定于分区的CSS样式？

**答案**

如果我们需要包含特定于分区的CSS，我们可以使用`{% style %}{% endstyle %}`标签来实现，这将允许我们使用Liquid代码。然而，除了样式标签外，我们还需要定义一个唯一的标识符，稍后将其作为选择器调用。我们可以使用`section`对象和`id`属性来实现这一点，它将为动态分区返回一个动态ID，或为静态分区返回一个分区文件名。我们可以通过访问*style标签*部分来回顾如何创建特定于分区的CSS。

请注意，所有四个项目的解决方案都可以在GitHub上找到：https://github.com/PacktPublishing/Shopify-Theme-Customization-with-Liquid/tree/main/Projects




