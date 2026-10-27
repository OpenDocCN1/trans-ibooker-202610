

## Assessments

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

