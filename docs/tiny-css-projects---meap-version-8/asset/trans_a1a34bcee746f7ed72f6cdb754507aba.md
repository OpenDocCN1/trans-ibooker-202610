

## 8
设计结账购物车

## 本章涵盖

- 条件化表单输入的样式
- 响应式表格
- 使用网格自动定位
- 数字格式化
- 通过媒体查询根据视口大小有条件地设置 CSS
- nth-of-type() 伪类

从食品、书籍到娱乐以及介于两者之间的所有内容，我们中的许多人会定期在线购买商品。这一体验的共同点是结账购物车。我们通过将商品添加到虚拟购物车或篮子中来做出选择，以便在最终购买前查看我们选择的商品。在本章中，我们将研究如何为结账购物车设计样式，使其在窄屏和宽屏上都能正常工作。

在本章中，我们将研究如何在窄屏和宽屏上处理表格。表格对于显示数据非常有用，然而，对于移动设备来说，样式化可能有点困难。因此，我们将研究窄屏的 CSS 解决方案。

我们首先处理一些主题。无论屏幕宽度如何，输入字段、链接或按钮等元素看起来都一样，因此我们将先对这些进行样式化。在组装 UI 的早期定义主题可以显著减少冗余代码并提高我们保持样式一致的能力，因此无论是结账购物车还是任何其他页面或应用程序，此过程都可以翻译到任意数量的设计中。

我们将然后专注于从窄到宽的布局。因为在窄屏（如手机）上，我们倾向于堆叠元素，并且随着变大，我们添加规则来利用我们可用的全部宽度。从移动端开始并随着屏幕变宽而增加样式，通常比从宽屏开始然后在变小时覆盖之前设置的布局元素更容易。我们将创建样式以适应 3 种屏幕尺寸：

- 窄屏（大多数手机）：最大宽度为 549 像素
- 中等（平板电脑和小屏幕）：介于 500px 和 955px 之间
- 宽屏（桌面和高分辨率平板电脑）：宽于 955px

图 8.1 显示了我们的起点以及完成时每个屏幕尺寸的输出效果。

图 8.1 小、中、大屏幕的开始和结束输出

## 8.1 开始

无论屏幕尺寸如何，我们都将使用相同的 HTML。我们将有一个样式表，并使用媒体查询来调整元素在不同屏幕尺寸下的外观。媒体查询用于有条件地将样式应用于元素；我们将在本章后面更详细地探讨这些。

我们的 HTML 包含两个部分，一个用于购物车，一个用于摘要，它们被包装在一个容器中，我们将在宽屏上使用该容器将各部分并排放置。购物车部分包含一个标题和一个包含购物车中每个商品的表格。摘要部分包含一个标题、一个定义列表和 2 个链接。图 8.2 显示了 HTML 元素的图表。

![](img/b56145aa9b751926a7aedbd7172bc818_214_0.png)

图 8.2 HTML 元素图

清单 8.1 是我们开始使用的 HTML 的缩写版本。代码提到了一个 JavaScript 文件。它存在于根据应用于输入字段的数量计算并插入总值，但不影响样式。一些 HTML 元素还添加了 ARIA（可访问富互联网应用程序），以确保我们的代码是可访问的。ARIA 提供了一个框架来添加属性，这些属性有助于识别代码本身未传达的功能、关系和交互，这些功能由辅助技术（如屏幕阅读器）使用。它对样式也没有任何影响，但我们始终希望确保我们的代码是可访问的，以便我们不阻止残障用户访问和使用我们的应用程序。

## 清单 8.1 起始 HTML

```
<body>
  <main>
    <h1>Checkout</h1>
```
```
<div class="section-container">
  <section>
    <h2>My Cart</h2>
    <table>
      <thead>
        <tr>
          <th>Image</th>
          <th>Item</th>
          <th>Unit Price</th>
          <th>Quantity</th>
          <th>Total</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>
            <img src="./img/grapes.jpg" width="75" loading="lazy" alt="Red grapes">
          </td>
          <td data-name="Item">Red Grapes, 1lb</td>
          <td data-name="Unit Price">$ 3.23</td>
          <td data-name="Quantity">
            <input name="grapes" type="number" aria-label="Pounds of grape baskets" min="0" max="99" id="grapes" value="1" aria-errormessage="grapeError">
            <p class="error" id="grapeError" role="alert" aria-live="polite">Must be a whole number between 0 and 99</p>
          </td>
          <td data-name="Total">
            <!-- value calculated & inserted by JS -->
          </td>
          <td>
            <button type="button" class="destructive">
              <img width="24" src="./img/icons/remove.svg" alt="remove grapes">
            </button>
          </td>
        </tr>
        ...
      <tfoot>
        <tr>
          <th colspan="4" scope="row">Total:</th>
          <td id="total">
            <!-- value calculated & inserted by JS -->
          </td>
        </tr>
      </tfoot>
    </table>
  </section>

  <section class="summary">
    <h2>Summary</h2>
    <dl>
      <dt>Number of Items</dt>
      <dd id="itemQty">
        <!-- value calculated & inserted by JS -->
      </dd>
    </dl>
```
```
<dt>Order Total</dt>
<dd id="orderTotal">
<!-- value calculated & inserted by JS -->
</dd>

<dt>Shipping</dt>
<dd>$ 5.00</dd>

<dt>Total</dt>
<dd id="grandTotal"></dd>
</dl>
<div class="actions">
<a href="" type="button" class="button primary">
Proceed to Checkout
</a>
<a href="" type="button" class="button secondary">
Continue Shopping
</a>
</div>
</section>
</div>
</main>
</body>
```

与前几章一样，起始和完成项目的代码都可以在 github 仓库中找到：https://github.com/michaelgearon/Tiny-CSS-Projects/tree/main/chapter-08。

## 8.2 主题化

尽管我们的布局有 2 个明确定义的区域（购物车和摘要），并且需要在不同屏幕尺寸下工作，但有一些样式无论它们位于何处或屏幕尺寸如何都不会改变。这些包括

- 使用的字体
- 按钮和链接样式
- 输入和错误消息样式
- 标题大小和颜色

这些类型的样式可以称为我们的主题，为了在页面上保持一致，我们通常希望一次编写它们并在任何地方应用。让我们从我们的字体开始。

### 8.2.1 排版

目前我们的 `font-family` 是浏览器的默认值。在我的例子中它是 Times New Roman，但这可能因您的操作系统、浏览器和系统设置而异。图 8.3 显示了默认字形和开发工具指示正在使用 Times New Roman。在大多数浏览器中，右键单击页面并选择检查将打开开发工具。由于 font-family 未被明确设置，一些浏览器会隐藏它，您可能需要通过计算属性列表顶部的复选框或通过浏览器工具设置来兴奋地告诉浏览器显示所有属性。

## Checkout

## My Cart

![](img/b56145aa9b751926a7aedbd7172bc818_217_0.png)

图 8.3 Times New Roman 的默认浏览器

我们将不再使用 Times New Roman，而是从 Google Fonts 导入 Raleway。然后我们将创建一个针对 body 标签的规则，以应用该字体。为此我们将使用 font-family 属性，将其值指定为 "Raleway"。我们还应用了 sans-serif 作为后备。如果 Raleway 无法加载，我们确保将使用一个可比的替代方案（Raleway 是一种无衬线字体）。通过将样式应用于 body 标签，我们充分利用了级联，字体将被应用于任何地方。

应用了 font-family 后，我们现在将设置文本的默认颜色。也在 body 上，使用 color 属性为文本分配颜色。我们将使用值 #171717，它看起来几乎是黑色。我们在这个设计中不使用黑色，因为它是一个非常柔和的设计，而纯黑色可能相当刺眼。

接下来，我们将处理我们的数字。字体系列默认具有旧式或现代数字。两者之间的区别在于数字相对于中线和基线的对齐方式，如图 8.4 所示。

![](img/b56145aa9b751926a7aedbd7172bc818_217_1.png)

图 8.4 旧式与现代数字

旧式数字的部分数字会超出基线之上和之下，而现代数字则不会。因为我们正在创建一个购物车，我们希望堆叠数字以显示它们相加形成总和，我们希望使用现代样式数字，以便它们更整齐地对齐。然而，Raleway（我们为页面选择的 font-family）默认使用旧式数字。为了使我们的字形使用现代数字，我们可以

