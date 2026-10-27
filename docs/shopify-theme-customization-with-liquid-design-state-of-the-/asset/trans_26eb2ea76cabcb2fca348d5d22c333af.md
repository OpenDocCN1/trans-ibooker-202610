
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

