## 第二章 线性表

线性表是最基本、最常用的一种线性结构。线性表由n（n≥0）个数据元素组成的有穷序列，该序列中所有元素具有相同的数据类型。其中数据元素可以由若干个数据项组成。

线性表的特点是：在数据元素的非空有限集合中，除第一个元素外，其它数据元素有且仅有一个直接前驱；除最后一个元素外，其它数据元素有且仅有一个直接后继。

- 顺序表
- 链表

下面分别介绍线性表的顺序存储和链式存储。

头部消息标记（包括现有的 `<h1>` 标签及其正下方的相应 `<div>`）。

```html
<div class="jumbotron p-4">
    <div class="container">
        <h1 class="jumbotron-heading">Rango says...</h1>
        <div>
            <h2 class="h2">
                {% if user.is_authenticated %}
                    howdy {{ user.username }}!
                {% else %}
                    hey there partner!
                {% endif %}
            </h2>
            <strong>{{ boldmessage }}</strong>
        </div>
    </div>
</div>
```

对于 `<div>` 容器，我们应用了 jumbotron 和 p-4 类。p-4 类控制 [间距](https://getbootstrap.com/docs/4.2/utilities/spacing/)，即 jumbotron 周围的内边距。尝试将内边距改为 p-6 或 p-1，看看会发生什么！你还可以通过专门设置 pt、pb、pr 和 pl 来控制上、下、左、右的空间，而不仅仅是设置 p。

### 网站样式练习

![](img/75169a27287f7c945bf2cbf7dede5251_222_0.png)

更新所有其他模板，使页面标题封装在 jumbotron 中。这将使整个应用具有一致的外观，这对于专业设计的网站至关重要。

不要忘记更新注册包使用的模板！

成功添加 jumbotron 之后，我们可以继续到两栏布局。这里我们借鉴了 [相册](https://getbootstrap.com/docs/4.2/examples/album/) 布局。虽然它有三列（称为卡片），但我们只需要两列。大多数（即便不是全部）CSS 框架都使用由总共 12 列组成的 [网格布局](https://getbootstrap.com/docs/4.2/layout/grid/)。如果你检查相册布局的 HTML 源代码，会发现在一行中有一个 `<div>` 用于设置卡片的大小。这个 `<div>` 是 `<div class="col-md-4">`，后面跟着 `<div class="card mb-4 shadow-sm">`。这将每张卡片的长度设置为相对于宽度的 4 个单位（共 12 个单位）（4 乘以 3 等于 12）。由于我们需要两张卡片（一张用于最受欢迎的页面，一张用于最受欢迎的类别），所以我们可以将 4 改为 6（即 50%，2 乘以 6 等于 12）。要实现这一点，请再次更新 `index.html` 模板。这一次，我们要替换现有的用于容纳最受欢迎类别和最受欢迎页面的 `<div>` 元素。

```html
<div class="container">
    <div class="row">
        <div class="col-md-6">
            <div class="card mb-6">
                <div class="card-body">
                    <h2>Most Liked Categories</h2>
                    <p class="card-text">
                    {% if categories %}
                    <ul>
                    {% for category in categories %}
                        <li>
```




```html
<a href="{{ page.url }}">{{ page.title }}</a>
</li>
{% endfor %}
</ul>
{% else %}
<strong>There are no pages present.</strong>
{% endif %}
</p>
</div>
</div>
</div>
</div>
</div>
```

更新模板后，重新加载页面——现在应该看起来好多了，但列表项的呈现方式还不是最佳。再说一次，它看起来很基础。我们肯定可以让它看起来更好！

让我们使用 [Bootstrap 提供的列表组样式](https://getbootstrap.com/docs/4.2/components/list-group/) 来改进超链接的呈现方式。我们可以通过将两个 `<ul>` 元素改为 `<ul class="list-group">`，并将我们刚刚添加的每个 `<li>` 元素包含一个类 `<li class="list-group-item">` 来轻松做到这一点。完成这些步骤后，重新加载索引页面。现在看起来如何？它应该与 [下图](#) 类似。

> [16](https://getbootstrap.com/docs/4.2/components/list-group/)
