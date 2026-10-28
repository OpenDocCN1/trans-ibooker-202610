

## 8.1 URLs 和多个 Django 应用

本书重点介绍单个 Django 应用 Rango 的开发。然而，你可能会发现自己正在处理一个使用多个应用同时工作的 Django 项目。这意味着你可能 literally 有数百个潜在的 URL 需要引用。这个场景引出了一个问题：*我们如何组织这些 URL？* 两个应用可能有同名的视图，从而存在潜在冲突。

Django 提供了为项目中使用的每个单独应用 *命名空间* URL 配置模块² 的能力。**正如我们之前所做的那样**，在应用的 `urls.py` 模块中添加一个 `app_name` 变量就足够了。下面示例将 Rango 应用的命名空间指定为 `rango`。

```python
from django.urls import path
from rango import views

app_name = 'rango'

urlpatterns = [
    path('', views.index, name='index'),
    path('about/', views.about, name='about'),
    path('category/<slug:category_name_slug>/add_page/', views.add_page, name='add_page'),
    ...
]
```

添加 `app_name` 变量意味着你从 `rango` 应用引用的任何 URL 都是这样完成的：

```html
<a href="{% url 'rango:about' %}">About</a>
```

其中 `url` 命令中的冒号将命名空间与实际的 URL 名称分隔开。

如果未指定 `app_name`，你将不得不仅使用映射名称来引用 URL，如下例所示。

```html
<a href="{% url 'about' %}">About</a>
```

当然，正如我们上面所讨论的，这种方法会打开潜在冲突的大门。如果有疑问，指定 `app_name` 是一个良好的实践！

## 8.2 处理重复

虽然你使用的几乎每个专业制作的网站都会有一系列重复的组件（例如页面头部、侧边栏和页脚），但为每个重复组件重复 HTML 并不是一个特别明智的处理方式。如果想更改网站头部的某个部分？你需要遍历每个页面并更改每个头部副本以满足需求。

与其花费（或浪费！）大量时间复制粘贴 HTML 标记，我们可以通过使用 Django 模板语言提供的模板继承来最小化 Rango 代码库中的重复。

使用继承在模板中的基本方法如下。

1. 识别每个页面中在整个应用中重复出现的部分（即头部栏、侧边栏、页脚、内容窗格）。有时，在纸上绘制不同页面的基本结构可以帮助你发现哪些组件是共用的。
2. 在基础模板中，提供基本页面的骨架结构，以及任何通用内容（即页脚中的版权声明、部分中出现的标志和标题）。然后定义几个块，这些块会根据用户查看的页面而有所变化。
3. 为应用的页面创建特定的模板——所有这些都继承自基础模板——并指定每个块的内容。

### 重复的 HTML 和基础模板

鉴于我们到目前为止创建的模板，应该很明显我们重复了相当多的 HTML 代码。下面，我们抽象掉了任何页面特定的细节，以显示我们在每个模板中重复的骨架结构。

```html
<!DOCTYPE html>
{% load staticfiles %}

<html>
  <head lang="en">
    <meta charset="UTF-8" />
    <title>Rango</title>
  </head>
  <body>
    <!-- Page specific content goes here -->
  </body>
</html>
```

暂时，让我们把这个简单的 HTML 页面作为应用的基础模板。保存此标记到 Rango 的 `base.html` 模板中。

### DOCTYPE 放在最前面！

请记住，`<!DOCTYPE html>` 声明**始终需要放在模板的第一行**。如果在第一行没有文档类型声明³，则从模板生成的页面可能不符合 W3C HTML 指南⁴。

### 模板块

现在我们创建了基础模板，我们可以添加模板标签来表示模板中哪些部分可以被继承它的模板覆盖。为此，我们将使用 `block` 标签。例如，我们可以在 `base.html` 中的基础模板中添加一个 `body_block`，如下所示：

```html
<!DOCTYPE html>
{% load staticfiles %}

<html>
    <head lang="en">
        <meta charset="UTF-8" />
        <title>Rango</title>
    </head>
    <body>
        {% block body_block %}
        {% endblock %}
    </body>
</html>
```

请记住，标准的 Django 模板命令由 {% 和 %} 标签表示。要开始一个块，命令是 `{% block <NAME> %}`，其中 `<NAME>` 是你希望创建的块的名称。你还必须确保使用正确的 `{% endblock %}` 命令关闭块，同样包含在 Django 模板标签中。

你还可以为块指定 *默认内容*，如果没有继承模板定义给定块，则会使用该内容（见[下面](#)）。通过在 `{% block %}` 和 `{% endblock %}` 模板命令之间添加标记来指定默认内容，就像下面的示例一样。

```django
{% block body_block %}
    This is body_block's default content.
{% endblock %}
```

当我们为每个页面创建模板时，我们将继承 `rango/base.html` 并覆盖 `body_block` 的内容。然而，你可以在模板中放置任意数量的块。例如，你可以为页面标题创建一个块，为页脚创建一个块，为侧边栏创建一个块，等等。块是 Django 模板系统的非常强大的功能，你可以了解更多关于它们的信息，请查看 [Django 的官方模板文档](https://docs.djangoproject.com/en/2.1/topics/templates/)。

### 提取通用结构

你应该始终努力为基础模板提取尽可能多的重复内容。虽然这样做可能很麻烦，但你在维护上节省的时间将远远超过前期投入的开销。

> *思考很痛苦，但比做大量繁重的工作要好！*

### 进一步抽象

现在你对 Django 模板中的块有了了解，让我们有机会进一步抽象我们的基础模板。重新打开 `rango/base.html` 模板并将其修改为如下所示。

```html
<!DOCTYPE html>
{% load staticfiles %}

<html>
    <head>
        <title>
            Rango -
            {% block title_block %}
                How to Tango with Django!
            {% endblock %}
        </title>
    </head>
    <body>
        <div>
            {% block body_block %}
            {% endblock %}
        </div>
        <hr />
        <div>
            <ul>
                <li><a href="{% url 'rango:add_category' %}">Add a New Category</a></li>
                <li><a href="{% url 'rango:about' %}">About</a></li>
                <li><a href="{% url 'rango:index' %}">Index</a></li>
            </ul>
        </div>
    </body>
</html>
```

从上面的示例中，我们在基础模板中引入了两个新功能。

- 第一个是名为 `title_block` 的模板块。这将允许我们为继承自基础模板的每个页面指定自定义页面标题。如果继承页面未覆盖该块，则 `title_block` 默认为 `How to Tango with Django!`， resulting in 完整的标题为 `Rango - How to Tango with Django!`。查看上面模板中 `<title>` 标签的内容即可了解其工作原理。
- 我们还将当前 `index.html` 模板中的链接列表——以及指向索引页面的新链接——包含在内，并将其放置在 `body_block` 块下方的 HTML `<div>` 标签中。这将确保链接存在于继承自基础模板的所有页面中。链接前面有一个 *水平线*（`<hr />`），为用户在 `body_block` 块的内容和链接之间提供视觉分隔。

## 8.3 模板继承

现在我们创建了一个带有块的基础模板，我们可以更新我们创建的所有模板，使其继承自基础模板。让我们从重构模板 `rango/category.html` 开始。

你可以通过首先删除所有重复的 HTML 代码，只留下特定于页面的 HTML 和模板标签/命令来实现。然后在模板开头添加以下代码行：

```django
{% extends 'rango/base.html' %}
```

`extends` 命令接受一个参数——要扩展/继承的模板（即 `rango/base.html`）。你提供给 `extends` 命令的参数应相对于项目的 `templates` 目录。例如，我们为 Rango 使用的所有模板都应扩展自 `rango/base.html`，而不是 `base.html`。

