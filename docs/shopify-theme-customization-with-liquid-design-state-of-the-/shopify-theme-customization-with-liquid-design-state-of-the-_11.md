# 探索 Shopify Ajax API

在前几章中，我们学习了 Shopify 和 Liquid 的基础知识，这为我们未来的开发奠定了坚实的基础。在建立了未来学习的良好基础后，我们学习了 Liquid 核心的工作方式。通过学习对象、标签和过滤器，我们了解了如何使用看似简单且不起眼的功能集来创建复杂的功能。最后，我们学习了如何使用各种输入类型设置，结合`sections`和`blocks`属性，在整个商店中创建易于配置的元素。

在这些章节中，我们学习了如何创建包含静态内容的元素。但是，如果我们希望动态更新页面内容该怎么办？这时 Shopify Ajax API 就能派上用场。在最后一章中，我们将介绍 Shopify Ajax API，说明其要求和限制，以及可能的使用场景。

在本章中，我们将涵盖以下主题：

- Shopify Ajax API 简介
- 使用 POST 请求更新购物车会话
- 使用 GET 请求检索数据

完成本章后，我们将理解 Shopify Ajax API 是什么，以及我们可以发出的请求类型，例如检索产品信息、将产品添加到购物车，甚至读取购物车当前内容。此外，我们还将通过处理之前的一些项目并使用 Ajax API 对其进行改进，来了解 Shopify Ajax API 的典型使用场景。

最后，我们将学习如何拉取并渲染基于 Shopify 算法自动生成的产品推荐列表，然后将普通搜索输入转换为商店所有者经常要求的前瞻搜索功能。

## 技术要求

虽然我们会解释每个主题并附上相关图示，但考虑到 Shopify 是一项托管服务，您需要联网才能完成本章中的步骤。

本章的代码可在 GitHub 上获取：https://github.com/PacktPublishing/Shopify-Theme-Customization-with-Liquid/tree/main/Chapter08。

虽然本章将包含每个主题的一些真实示例和使用场景，但您需要具备 Ajax 的基本理解和知识，才能完全跟上本章内容。

请注意，我们只会展示与 Shopify API 相关的示例和项目，而不是一般的 Ajax。有关 Ajax 的详细信息，请参阅 https://www.w3schools.com/js/js_ajax_intro.asp，该网站对 Ajax 做了很好的介绍。

本章的 Code in Action 视频可在此处找到：https://bit.ly/2VUZ7Qp

## Shopify Ajax API 简介

Ajax（即异步 JavaScript 和 XML）是一种方法，我们可以用它来与服务器交换少量数据并更新页面的某些部分，而无需重新加载整个页面。那么，Shopify Ajax API 到底是什么？

Shopify Ajax API 是一个 REST API 端点，我们可以通过它发送请求来读取或更新特定信息。例如，我们可以使用 GET 请求读取产品或当前购物车数据，也可以使用 POST 请求更新当前购物车的内容会话。

Shopify Ajax 是一个未经身份验证的 API，这意味着它不需要任何令牌或 API 密钥即可访问商店信息。Shopify 还为我们提供了一个经过身份验证的 API，名为**Shopify Admin API**，应用程序和服务使用它与 Shopify 服务器进行通信。

通过 Shopify API，我们可以访问大部分商店数据，其响应将返回 JSON 格式的数据，但我们无法读取客户和订单数据或更新任何商店数据——我们只能使用 Shopify Admin API 来执行这些操作。

请注意，Shopify 对 Ajax API 有一定的速率限制，以防止滥用（即向 Shopify 服务器发送无限制的请求）。其中一项限制是输入数组大小上限，目前限制为 250。假设我们希望拉取一个包含超过 1,000 件产品的集合中所有产品的信息。我们必须使用多个查询来实现这一点，因为每次查询最多只能获取 250 个产品。

> **提示：**
> 
> 为了保持内容简洁明了，这里不会列出所有速率限制。有关 Ajax API 速率限制的更多信息，请参阅 `https://shopify.dev/api/usage/rate-limits`。

现在我们已经熟悉了 Shopify Ajax API 的基本知识，接下来可以从实践的角度进一步了解 Ajax API。

## 使用 POST 请求更新购物车会话

前面我们提到，可以使用 POST 请求更新当前购物车会话。根据我们要执行的操作类型，我们可以将 POST 请求与以下购物车端点配合使用：

- `/cart/add.js`
- `/cart/update.js`
- `/cart/change.js`
- `/cart/clear.js`

虽然这听起来可能很简单，但它是当今电子商务商店的一个重要方面，因为用户期望无需刷新整个页面即可执行操作。





### `/cart/add.js` 端点

顾名思义，`/cart/add.js` 端点允许我们向购物车添加一个或多个产品变体，而无需刷新购物车。要执行此操作，我们需要创建一个名为 `items` 的数组，其中包含一个对象，该对象包含以下两个键：

- `id` 键，其值应包含我们要添加到购物车的变体 ID 的数字类型值。
- `quantity` 键，其值应包含我们要添加到购物车的数量的数字类型值。

如果我们需要包含多个变体，只需在 `items` 数组中追加多个对象即可：

```
items: [
  {
    id: 40065085407386
  },
  {
    id: 40065085603994,
    quantity: 5
  }
]
```

在上面的示例中，我们看到了一个包含两个对象的数组，这两个对象包含一组不同的变体 `id` 和 `quantity` 键。但请注意，第一个对象不包含 `quantity` 键。原因是 `quantity` 键完全是可选的，如果我们没有包含它，则默认 `quantity` 值为 1。

让我们看看如何在实际示例中使用它：

1. 你可能还记得，在 *第 4 章，深入 Liquid 核心：对象* 以及后来的 *第 5 章，深入 Liquid 核心：过滤器* 中，我们通过将额外的集合添加到集合模板中来处理 `Custom collection` 项目。然而，当前的功能是，如果我们点击 **添加到购物车** 按钮，当前表单流程会将相应的变体添加到购物车，并将我们重定向到购物车页面。让我们通过创建 POST 请求来改变这一点。

2. 正如我们所见，我们创建的集合表单已经包含我们需要的两个必要内容：提交按钮和存储在隐藏输入中的变体 ID。为了方便后续操作，我们先为提交按钮分配一个名为 `collection-submit` 的新类：

```liquid
{% form "product", product %}
  <input type="hidden" name="id" value="{{ product.first_available_variant.id }}" />
  <input type="submit" class="collection-submit" value="Add to Cart"/>
{% endform %}
```

3. 有了合适的选择器，我们现在可以在提交按钮上使用 `addEventListener` 来捕获点击事件，并将该对象传递给我们接下来要创建的函数：

```javascript
const addSelector = document.querySelectorAll(".collection-submit");
if (addSelector.length) {
  for (let i = 0; i < addSelector.length; i++) {
    addSelector[i].addEventListener('click', function(e) {
      e.preventDefault();
      addCart(this);
    });
  }
}
```

4. 在前面的示例中，我们创建了一个 `addSelector` 常量来捕获点击事件。通过使用 `preventDefault()`，我们取消了当前任何事件的流程，并将被点击元素的对象传递给了 `addCart` 函数。现在，让我们来看如何创建 `addCart` 函数：

```javascript
const addCart = (el) => {
  let formData = {
    'items': [
      {
        id: el.previousElementSibling.value
      }
    ]
```

我们首先创建了一个带有 `el` 参数的箭头函数，我们将之前点击的元素对象传递给它。在 `addCart` 函数内部，我们创建了一个局部变量，并在其中赋值了一个数组。这个数组包含一个对象，该对象包含我们想要添加到购物车的变体的 `id` 属性。

5. 考虑到我们之前将点击的对象传递给了箭头函数，我们使用 `previousElementSibling` 来选择正确的输入元素并相应地返回其值。现在我们已经准备好所有必要的资源，我们只需要使用 `fetch` 请求将数据 POST 到 Shopify 服务器，并更新当前的购物车会话：

```javascript
const addCart = (el) => {
  let formData = {
    'items': [
      {
        id: el.previousElementSibling.value
      }
    ]
  };

  fetch('/cart/add.js', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(formData)
  })
  .then(success => {
    console.log("Success:", success);
  })
  .catch((error) => {
    console.error('Error:', error);
  });
}
```

这样，我们就成功创建了一个功能完整的 Ajax API POST 请求，允许我们在不重新加载页面的情况下向当前购物车会话添加任意数量的商品。此外，我们还包含了 `then()` 和 `catch()` 方法，以便在控制台日志中返回成功和错误消息。

我们还学习了如何通过 `/cart/add.js` 端点将指定数量的特定产品添加到当前购物车会话中。*然而，如果特定产品上有某些我们希望带入购物车的订单项属性，该怎么办？*

我们只需包含一个额外的参数 `properties` 即可轻松解决此问题，该参数接受一个键值类型对象：

```
let formData = {
  'items': [
    {
      id: el.previousElementSibling.value,
      properties: {
        'Engraving message': 'Learning Liquid is fantastic!'
      }
    }
  ]
};
```

我们应该将键设置为等于订单项输入的名称或订单项属性的第一部分，而值应等于从输入中获取的值或订单项属性的第二部分。如果我们需要回顾订单项属性是如何工作的，可以重新查看 *第 4 章，深入 Liquid 核心：对象*，在 *使用全局对象* 部分下的 *产品定制* 子主题中，我们解释了订单项属性如何工作。

如果我们需要传递一个仅在管理员后台的订单部分可见的隐藏订单项，我们需要在键前添加一个*下划线*：

```
let formData = {
  'items': [
    {
      id: el.previousElementSibling.value,
      properties: {
        '_Engraving message': 'Learning Liquid is fantastic!'
      }
    }
  ]
};
```

如果我们决定使用 jQuery，我们可以让代码更加简洁：

```
jQuery.post('/cart/add.js', {
  items: [
    {
      quantity: 1,
      id: 40065085407386,
      properties: {
        '_Engraving message': 'Learning Liquid is fantastic!'
      }
    }
  ]
});
```

但是，我们应该检查当前主题是否已经包含了 jQuery 库。否则，我们应该避免向主题引入新的库。

通过同时介绍 JavaScript 和 jQuery 两种解决方案，我们现在可以确信自己能够运用这些技能编写出所需的 Ajax API 代码。*然而，如果我们不小心添加了超出所需数量的商品，并且需要更新产品数量，该怎么办？*

### `/cart/update.js` 端点

顾名思义，`/cart/update.js` 端点允许我们更新当前购物车会话中的订单项值。

虽然 `/cart/update.js` 的工作方式与 `/cart/add.js` 类似，但还是有一些明显的区别。例如，在 `/cart/add.js` 中，处理多个变体时我们必须创建一个单独的对象，而使用 `/cart/update.js` 时，我们只需创建一个对象：

```javascript
updates: {
  40065085407386: 5,
  40065085603994: 3
}
```

请注意，这里我们不再使用两组键值，而只使用一组，其中键由变体 ID 表示，数量值作为键值。此外，我们现在使用的是 updates 而不是 items。让我们创建一个函数来帮助我们测试新学到的知识：

```javascript
const updateCart = (el) => {
  let formData = {
    updates: {
      [el.previousElementSibling.value]: 5
    }
  };

  fetch('/cart/update.js', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(formData)
  })
    .then(success => {
      console.log("Success:", success);
    })
    .catch(error => {
      console.error('Error:', error);
    });
}
```


正如我们所见，更新购物车会话的通用代码与将商品加入购物车的代码类似，这并不令人意外。除了更新购物车内容之外，`/cart/update.js` 还允许我们将商品添加到当前购物车会话中。

通过使用 `/cart/update.js`，我们可以使用变体 ID 来标识要更新的变体，从而轻松更新购物车中每个商品的数量。*但如果我们想要更新的变体不在购物车中呢？* 这时 `/cart/update.js` 的替代功能便会触发，它会将选定的商品变体及其数量添加到购物车，从而更新当前购物车会话。

例如，在之前的 `updateCart` 函数中，我们将 `quantity` 值设置为静态值 5。无论我们调用该函数多少次，购物车中任何变体的总数量都不会超过 5。因此，我们建议始终使用 `/cart/update.js` 更新购物车中已有的商品，并使用 `/cart/add.js` 向购物车添加更多商品。

至此，我们已经学会了如何更新当前购物车会话中的订单项。然而，你或许还记得*第4章 深入 Liquid 核心：对象*中，位于*使用全局对象*一节中的*产品定制*子主题中，我们了解到可以使用订单项来实现不同类型的定制。因此，如果定制内容不同，相同的产品变体会被归入不同的订单项中。

尽管这些产品可能位于不同的订单项中，但它们的变体 ID 相同。*那么，如果我们在三个不同的订单项上运行 `/cart/update.js` 来更新特定变体，会发生什么？*

`/cart/update.js` 端点会成功执行其操作。然而，由于它不知道我们要更新哪一行项目，它只会更新第一个具有匹配变体 ID 的订单项，然后停止。它不会更新具有相同变体 ID 的其他任何匹配项。*但如果我们要更新的是某个特定的订单项，而不是第一个匹配项呢？*

### /cart/change.js 端点

`/cart/change.js` 端点的工作方式与 `/cart/update.js` 端点类似，它允许我们更新当前购物车会话中的订单项。然而，两个关键的区别是：我们一次只能修改一个订单项，而且（更重要的是）我们可以精确指定要修改哪一行项目。

与 `/cart/add.js` 端点类似，`/cart/change.js` 端点也使用一个包含两个键值对的对象——一个用于标识订单项，一个用于分配所需数量：

```
{
  'id': 40065085407386,
  'quantity': 7
}
```

虽然使用 `id` 和变体 ID 来标识订单项不会导致任何错误，但这并不能解决我们的问题，因为购物车中可能存在多个具有相同变体 ID 的订单项。为了解决这个问题，我们可以使用 `line` 属性来标识我们想要更改的具体订单项：

```
{
  'line': 3,
  'quantity': 7
}
```

`line` 值基于当前购物车会话中订单项的索引位置，基准值从 1 开始。例如，如果购物车中有四个商品，而我们要更新第三个位置的订单项，则可以将 `line` 值设置为 3，如我们之前的示例所示。请注意，`/cart/change.js` 端点最常见的用途是轻松更新购物车页面中每个订单项的数量。

按照以下步骤成功实现 `/cart/change.js` 端点：

1. 如前所述，要成功使用 `/cart/change.js` 端点，我们需要两样东西：数量值（我们可以快速从修改的输入值中获取）以及订单项的当前位置。要确定订单项的位置，我们可以使用 JavaScript 的 `indexOf()` 方法。或者，如果数量输入位于 Liquid `for` 循环内，我们可以引入一个数据属性并将其值设置为 `forloop.index`。这里我们使用第二种方法，将 `forloop.index` 作为数据属性添加：

```
<input type="number" name="quantity" value="0" data-quantity-item="{{ forloop.index }}"/>
```

2. 在确保所有必要属性都就位后，我们只需要使用 `addEventListener` 来监听输入框上的 `change` 事件，然后将该对象传递给 `changeCart()` 箭头函数：

```
const changeCart = (el) => {
  let formData = {
    line: el.dataset.quantityItem,
    quantity: el.value
  };

  fetch('/cart/change.js', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(formData)
  })
  .then(success => {
    console.log("Success:", success);
  })
  .catch((error) => {
    console.error('Error:', error);
  });
}
```

`changeCart()` 箭头函数与我们之前创建的函数类似。唯一的区别是，现在我们使用 `/cart/change.js` 端点，并且不再使用静态键值对。相反，我们从之前传递的对象中获取这两个值。

虽然我们可以通过将数量值设置为 0，使用 `/cart/update.js` 和 `/cart/change.js` 从购物车中移除商品，但我们必须手动将每个订单项的数量调整为 0。但如果我们想要一种单击即可清空整个购物车的简便方法呢？

使用前面的代码示例，我们已成功地从当前购物车会话和 DOM 中移除了所有商品。请注意，还有许多额外需要处理的细节，例如清除价格、移除购物车表格以及显示购物车为空的提示消息。不过，为了保持本书简洁切题，我们不会深入讨论这些内容，但你可以自由地（并且建议你）持续升级前面的代码，因为你会从中受益。

到目前为止，我们已经学习了如何使用 `/cart/add.js` 端点将商品添加到当前购物车会话，使用 `/cart/update.js` 和 `/cart/change.js` 更新现有的订单项，以及使用 `/cart/clear.js` 端点清空当前购物车会话。然而，正如我们所见，虽然我们可以轻松地添加、更新甚至清空当前购物车会话中的商品，但仍需重新加载页面才能看到具体结果，例如更新页眉中购物车图标旁的商品计数器，或更新商品数量时的订单项价格。

虽然我们可以这样做，但可以简单地通过我们要添加到购物车的商品数量来递增商品计数器。实现这一目标的更直接的解决方案是使用 GET 请求并结合 Shopify Ajax API，这将允许我们从 Shopify 服务器检索各种数据，包括当前购物车会话中的商品数量。

## 使用 GET 请求检索数据

如前所述，使用 GET 请求，我们可以从 Shopify 服务器获取所有类型的数据，但客户和订单信息除外，这些信息只能通过经过身份验证的 Shopify Admin API 访问。根据我们要执行的操作类型，我们可以将 GET 请求与以下端点结合使用：

- `/cart.js`
- `/products/{product-handle}.js`
- `/recommendations/products.json`
- `/search/suggest.json`

GET 请求是一种非常强大的方法，我们通常会将其与 POST 请求结合使用，在对当前购物车会话做出更改后检索数据。然而，我们也可以使用 GET 请求来检索和创建复杂的功能，正如我们接下来要了解的那样。

### /cart.js 端点

`/cart.js` 端点，顾名思义，允许我们访问当前购物车会话，并检索有关购物车以及购物车内商品的所有信息。我们可以使用它来动态更新购物车页面，甚至为商店创建购物车抽屉，从而显著改善购买流程。让我们来看一下：

1. 我们可以使用以下 `fetch` 方法检索购物车信息：

```
fetch('/cart.js')
  .then(response => response.json())
  .then(data => {
    console.log(data);
  });
```

请注意，成功的 GET 请求的响应是一个 JSON 对象。以下示例展示了使用前面的代码获取购物车数据时收到的响应：

```
{
  "token": "fd2d9bc86cfa72228b4de6bff52fe915",
  "note": null,
  "attributes": {},
  "original_total_price": 78594,
  "total_price": 78594,
  "total_discount": 0,
  "total_weight": 0,
  "item_count": 9,
  "items": [],
  "requires_shipping": true,
  "currency": "USD",
  "items_subtotal_price": 78594,
  "cart_level_discount_applications": []
}
```

正如我们所见，JSON 对象分别提供了购物车和每个产品的所有必要信息。请注意，变体信息可以通过 `items` 数组对象访问，我们已对其进行了精简以保持内容简洁。




2. 既然我们已经学会了如何获取当前购物车会话信息，就可以将其与我们之前处理过的针对 `/cart/add.js` 的 POST 请求结合使用，从而确保每次我们向购物车添加新产品时，购物车计数器都能正确更新：

```
fetch('/cart/add.js', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(formData)
})
.then(success => {
  console.log("Success:", success);
  fetch('/cart.js')
  .then(response => response.json())
  .then(data => {
    document.querySelector("[data-cart-count]").innerHTML = data.item_count;
  });
})
.catch((error) => {
  console.error('Error:', error);
});
```

现在，我们已经掌握了从当前购物车会话中获取不同类型信息所需的所有知识。但请注意，购物车响应中的价格值只是纯字符串，并没有货币格式。

3. 例如，假设我们希望每次更新产品数量时，都更新购物车页面上的总价。首先，我们将使用 `fetch` 方法获取总价数值：

```
fetch('/cart.js')
.then(response => response.json())
.then(data => {
    console.log(data.total_price);
});
```

虽然我们成功获取了价格，但得到的只是一个未格式化的字符串值，对我们来说并不是很有用：

```
78594
```

4. 解决这个问题最简单的方法是查看主题开发者是如何在整个主题中定义货币格式化辅助函数的。我们通常可以在主题的 master js 文件中找到它。在我们的例子中，这个文件就是 `theme.js`。

在确定我们需要的关键字之后，我们只需要将格式化应用于我们想要格式化的数值：

```
fetch('/cart.js')
.then(response => response.json())
.then(data => {
    console.log(theme.Currency.formatMoney(
        cart.total_price, theme.moneyFormat));
});
```

请注意，我们在上一个示例中使用的格式化在大多数情况下无需任何修改即可正常工作。不过，我们可能需要对特定主题做一些调整——这完全取决于主题开发者是如何定义该函数的。

之前，我们学习了如何获取当前购物车会话的信息以及购物车中任何产品的数据。但是，如果我们想更直接地获取产品信息呢？

### `/products/{product-handle}.js` 端点

`/products/{product-handle}.js` 端点是一个简单的端点，我们可以将其与 GET 请求结合使用，轻松获取商店中任何产品的信息。与 `/cart.js` 端点类似，`/products/{product-handle}.js` 相对容易使用，因为它只需要我们提供要获取数据的产品 handle：

```
const getProduct = (handle) => {
  fetch(`/products/${handle}.js`)
    .then(response => response.json())
    .then(product => {
      console.log(product.id);
    });
}
```

从上述示例中返回的值将包含产品 ID，我们将在下面的示例中使用它。

此端点最常见的用途是实现点击功能，例如快速查看功能，在这种情况下，我们需要动态加载大量产品信息，以避免使 DOM 变得混乱并拖慢商店速度。

### `/recommendations/products.json` 端点

`/recommendations/products.json` 端点允许我们根据 Shopify 算法，为所选产品获取推荐产品的 JSON 对象列表，我们可以利用它来构建动态推荐区块。

通过此端点，我们可以使用三个参数，其中一个是必填参数，另外两个是可选参数：

- `product_id` 参数是一个必填参数，其值应设置为我们要获取推荐列表的产品的 ID。请注意，产品 ID 与变体 ID 不同。它们是我们可以通过 `product` 对象获取的两个不同属性。
- `limit` 参数是一个可选参数，允许我们选择每个请求最多接收的推荐产品数量。由于 Shopify 的限制，每个请求最多只能获取 10 个推荐产品。如果我们不设置 `limit` 参数，这就是默认值。
- 最后但同样重要的是 `section_id` 参数，它虽然是可选的，但非常有趣，因为它允许我们改变将收到的响应类型。通过将区块的 ID 作为 `section_id` 参数值，我们可以选择希望在其中渲染推荐产品的父元素。更重要的是，我们还可以将 JSON 响应更改为 HTML 字符串，然后将其与 `recommendations` 对象结合使用，动态输出推荐产品。

现在我们已经熟悉了可以与 `/recommendations/products.json` 端点一起使用的所有属性，是时候看看它们在实际中的应用了。

在下面的示例中，我们使用了 `fetch` 请求，并配对了 `/recommendations/products.json` 端点，来生成一个 JSON 对象列表并将其输出到控制台日志中：

```
const productRecommendations = (productId, limit) => {
  fetch(`/recommendations/products.json?product_id=${productId}&limit=${limit}`)
    .then(response => response.json())
    .then(products => {
      console.log(products);
    });
}
```

正如我们所见，获取推荐产品的 JSON 对象非常简单，因为我们现在唯一需要做的就是将产品 ID 传递给 `productId` 参数。你可能还记得，`limit` 参数是可选的，如果不包含该参数，它将默认为最大值 10。现在，让我们看看如何包含 `section_id`，并学习如何获取 HTML 字符串。

在修改 `fetch` 请求以实现此目的之前，我们需要做一些具体的准备工作。我们需要做的第一件事是在 `Sections` 目录中创建一个新的区块。在我们的示例中，我们将其命名为 `recommended-products`。

因为我们的产品页面模板中已经有一个推荐产品区块，而我们创建新区块只是为了了解其工作原理，所以让我们将这个新区块放在 `theme.liquid` 布局文件的底部，就在 `</body>` 标签上方。现在我们已经创建了区块文件并成功引入它，接下来必须熟悉 `recommendations` 对象。

顾名思义，`recommendations` 对象允许我们从产品推荐列表中获取产品。不过，这个特定对象只能与 `/recommendations/products` 端点结合使用。

正如我们所见，`recommendations` 对象使用起来相对简单，因为它只包含三个属性：

- `performed` 属性返回一个布尔值，取决于我们是否将 `recommendations` 对象放置在通过组合 `recommendations` 端点和必要参数来渲染其内容的区块内。
- `products_count` 属性为我们提供推荐列表中产品数量的数值。
- 最后但同样重要的是，`products` 属性允许我们获取推荐产品对象的数组。我们可以将 `products` 属性与 `for` 标签结合使用，输出结果的方式与我们在第 5 章《深入 Liquid 核心：过滤器》中为“自定义集合”项目所做的相同。

让我们回到创建的 `recommended-products` 区块文件，并使用 `recommendations` 对象输出推荐产品数组：

```
<div class="product-recommendations">
  {% if recommendations.performed %}
    {% if recommendations.products_count > 0 %}
      {% for product in recommendations.products %}
        <div class="product">
          <a href="{{ product.url }}">
            <img class="product__img" src="{{ product.featured_image | img_url: '300x300' }}" alt="{{ product.featured_image.alt }}" />
            <p class="product__title">{{ product.title }}</p>
            <p class="product__price">{{ product.price | money}}</p>
          </a>
        </div>
      {% endfor %}
    {% endif %}
  {% endif %}
</div>
```




至此，我们已经为未来的推荐列表创建了一个合适的布局。然而，如果我们在主题上预览结果，我们会注意到该部分在 product-recommendations div 内没有渲染任何内容。正如我们之前提到的，recommendations 对象只能与 recommendations 端点结合使用，所以让我们看看如何使用该端点来生成必要的 HTML 字符串，以输出推荐产品列表。

为了实现这一点，我们需要对之前的 fetch 请求做一些调整：

- 1. 我们需要做的第一件事是为 productRecommendations 函数包含额外的参数，我们将把 section ID 值传递给它。此外，我们还需要将 section_id 参数及其值包含到 fetch URL 中。
- 2. 第二个也是更重要的步骤是从 fetch URL 中移除 .json。否则，我们将无法检索到 JSON HTML 字符串。
- 3. 最后但同样重要的是，我们需要在第一个 then 方法中将 .json() 替换为 .text()。

此时，我们已经拥有了检索 JSON HTML 字符串所需的所有元素。让我们通过在控制台日志中调用产品来测试一下：

```
const productRecommendations = (productId, limit,
  sectionId) => {
  fetch(`/recommendations/products?product_id=${productId}&limit=${limit}&section_id=${sectionId}`)
  .then(response => response.text())
  .then(products => {
    if (products.length > 0) {
      console.log(products);
    }
  });
}
```

然而，在我们测试之前，我们需要向函数传递三个值：

- 对于 `productId`，我们可以使用在学习 `/products/{product-handle}.js` 端点时检索到的产品 ID 值。或者，我们可以在任何产品模板中使用 `product.id` 并复制我们收到的值。
- 对于 `limit`，我们可以使用任何不超过 10 的数字值，这是我们可以作为响应接收的最大产品数量。
- 对于 `sectionId`，我们应该包含一个字符串值，等于我们希望在其中显示推荐产品的 section 名称。在我们的例子中，该值是 `recommended-products`。

以下是将三个值传递给函数的示例：

```
productRecommendations(6796616663194, 3, "recommended-products");
```

如果我们预览重复的主题并检查之前 `fetch` 函数中的控制台日志，我们会看到我们已经成功检索到了推荐产品的 JSON HTML 字符串值。

既然我们已经确认一切正常，剩下唯一要做的事情就是使用检索到的值并输出推荐产品列表：

```
const productRecommendations = (productId, limit, sectionId) => {
  fetch(`/recommendations/products?product_id=${productId}&limit=${limit}&section_id=${sectionId}`)
    .then(response => response.text())
    .then(products => {
      if (products.length > 0) {
        document.querySelector(".product-recommendations").innerHTML = products;
      }
    });
}
```

至此，我们成功学会了如何根据之前在 `recommended-products` 部分中定义的布局来渲染推荐产品列表。此外，产品列表将根据 Shopify 的算法自动更新。

```
if (productSuggestions.length > 0) {
    console.log(productSuggestions);
}
});
}
```

正如我们所见，基于搜索查询检索预测搜索结果相对简单，因为我们现在唯一需要做的就是将所需的值传递给我们的函数：

```
const searchSelector = document.querySelectorAll(".search-bar__input");
if (searchSelector.length) {
    for (let i = 0; i < searchSelector.length; i++) {
        searchSelector[i].addEventListener('input',
            function(e){
                e.preventDefault();
                predictiveSearch(this.value, 4,
                    "product,page,article,collection");
            });
    }
}
```

通过在搜索字段中输入搜索查询，我们会注意到，在控制台日志中，我们已经成功检索到了一个组合列表，其中包含最多四个产品、页面、文章或集合的 JSON 对象，这些对象部分或完全匹配我们的搜索查询。现在唯一剩下的事情就是使用响应值在 DOM 中生成结果。

使用 `/products/{product-handle}.js` 端点时，我们有一个参数允许我们检索 JSON HTML 字符串，从而轻松地将结果输出到 DOM 中。然而，`/search/suggest.json` 端点并非如此；要渲染这些结果，我们需要使用 JavaScript 来创建我们需要的布局和功能。为了保持简洁明了，我们不会在本书中介绍这部分内容。不过，我们建议完成这个项目，因为这将是一个很好的练习，可以帮助你巩固目前所学的一切。

有关预测搜索参数及其一般要求和限制的更多信息，请查阅 https://shopify.dev/api/ajax/reference/predictive-search。

## 总结

在整个最后一章中，我们熟悉了 Shopify Ajax API，并了解了不同类型的用例。首先，我们学习了如何使用 `/cart/add.js` 端点升级当前的购买流程，通过该端点，我们可以直接将任意数量的产品、数量和行项目自定义（无论是公开还是私有）添加到当前购物车会话中。

通过学习如何处理 `/cart/change.js` 端点，我们获得了创建包含特定产品和数量的功能（例如自动赠品或追加销售功能）所需的知识。结合使用 `/cart/update.js` 和 `/cart.js` 端点，我们学习了如何动态更新购物车内容并检索它。然后，我们可以利用这一点来创建购物车抽屉功能。

此外，我们学习了如何使用 `/products/{product-handle}.js` 端点检索自动推荐产品列表，并将其内容渲染到我们选择的部分中。

最后，我们了解了 `/search/suggest.json` 端点，它允许我们创建预测搜索功能，这是商店所有者最常要求的功能之一。

从本书一开始，我们就共同努力突破知识的极限，并建立起扎实的理解流程，这将帮助我们在成为 Shopify 专家的道路上不断前进。虽然我们没有涵盖每一段 Liquid 代码，但我们参与了一些令人兴奋的项目，从中我们学到了更有益的东西。我们的目标不仅仅是创建一个列出所有不同方法和属性的清单（这些我们随时可以通过查阅 Shopify 文档找到），而是学习 Shopify 和 Liquid 的工作原理。

虽然可以说，通过我们在这里获得的知识，我们应该已经准备好独立开始处理 Shopify 主题，但请注意，我们的冒险并没有结束——它才刚刚开始。

Shopify 是一个不断发展的平台，它要求我们及时了解所有最新的公告和方法。幸运的是，Shopify 提供了各种社区，以进一步提升我们的知识，或在各种主题上获得其他 Shopify 专家的帮助。最后但同样重要的是，我们有一个 Discord 频道可供使用，在那里我们可以与其他开发者交流，既可以在需要时获得帮助，也可以与其他开发者分享我们的知识：`https://discord.gg/shopifydevs`。

## 延伸阅读

- Shopify 官方文档：https://shopify.dev/
- Shopify 速查表：http://cheat.markdunkley.com/
- 开发者更新日志：https://shopify.dev/changelog
- 官方社区：https://community.shopify.com/
- Twitter 公告：https://twitter.com/shopifydevs
- Shopify 开发者 YouTube 频道：https://www.youtube.com/channel/UCcYsEEKJtpxoO9T-keJZrEw
- Shopify 官方博客，获取有关 Shopify 世界的最新信息：https://www.shopify.com/partners/blog




