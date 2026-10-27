

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

## `/products/{product-handle}.js` 端点

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

## `/recommendations/products.json` 端点

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

