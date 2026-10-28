# 8. 使用模板

到目前为止，我们已经为Rango应用中的不同页面创建了几个HTML模板。当你创建每个额外的模板时，你可能会注意到大量的HTML代码是重复的。这感觉有点奇怪，或者有点浪费吗？

确实如此！每当你重复类似的代码时，你就违反了[DRY原则](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)——该原则指出：**不要重复自己！** 你可能已经注意到，在我们目前编写的模板中，我们使用*硬编码*的URL路径来引用不同的页面。这是不好的实践，因为路径终究会随着时间而改变。

综合来看，重复和硬编码将导致维护噩梦。如果我们想对网站的整体结构进行更改或更改URL路径，你将不得不修改*所有*包含我们想更改的结构或URL的模板。如果你的网站只有几个页面，这可能还可以处理。但是，如果你的网站有数百个页面呢？更新所有内容需要多长时间？你有多大的可能在某个地方犯错？

幸运的是，Django开发人员已经考虑了如何解决这类问题并提供了解决方案。在本章中，我们将使用*模板继承*来解决第一个问题，使用*URL模板标签*来解决第二个问题。我们先从解决第二个问题开始。

## 8.1 在模板中使用相对URL

到目前为止，我们一直在模板中直接编写我们想要显示的页面或视图的URL，即 `<a href="/rango/about/">About</a>`。这种URL硬编码意味着，如果我们更改 `urls.py` 中的URL映射，那么我们也必须更改模板中所有相关的URL引用。首选的方法是使用模板标签 `url` 来在 `urls.py` 模块中查找*URL映射名称*，并动态插入匹配的URL路径。

在模板中包含相对URL非常简单。要引用*关于*页面，我们将在模板中插入以下行：

```
<a href="{% url 'rango:about' %}">About</a>
```

Django模板引擎会在任何 `urls.py` 模块中查找具有 `name` 属性设置为 `about`（`name='about'`）且 `app_name` 设置为 `rango` 的URL模式，然后对URL执行反向查找。这意味着如果我们更改 `urls.py` 中的URL映射，就不必逐个检查引用它们的模板。

也可以直接引用视图，来引用没有指定名称的URL模式，如下所示。

```
<a href="{% url 'rango.views.about' %}">About</a>
```

在这个例子中，我们必须确保应用 `rango` 在 `views.py` 模块中包含视图 `about`。在你的应用的 `index.html` 模板中，你会注意到有一个参数化的URL模式（`show_category()` URL/视图将 `category.slug` 作为参数）。要处理此情况，你可以在模板中向 `url` 模板标签传递URL/视图的名称以及slug，如下所示：

```
{% for category in categories %}
    <li>
        <a href="{% url 'rango:show_category' category.slug %}">
            {{ category.name }}
        </a>
    </li>
{% endfor %}
```

在你匆匆去用相对URL更新所有模板中的所有URL之前，我们需要通过使用继承来重构我们的模板，以去除过多的重复。





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





然后我们可以进一步修改 `category.html` 模板，使其看起来像下面这个完整示例。

```
1 {% extends 'rango/base.html' %}
2 {% load staticfiles %}
3
4 {% block title_block %}
5     {% if category %}
6         {{ category.name }}
7     {% else %}
8         Unknown Category
9     {% endif %}
10 {% endblock %}
11
12 {% block body_block %}
13     {% if category %}
14 <h1>{{ category.name }}</h1>
15     {% if pages %}
16 <ul>
17         {% for page in pages %}
18             <li><a href="{{ page.url }}">{{ page.title }}</a></li>
19         {% endfor %}
20 </ul>
21     {% else %}
22 <strong>No pages currently in category.</strong>
23     {% endif %}
24
25     <a href="{% url 'rango:add_page' category.slug %}">Add Page</a> <br />
26     {% else %}
27     The specified category does not exist.
28     {% endif %}
29 {% endblock %}
```

## 练习

现在你已经完成了本章的第一部分，有几个练习可以帮助你巩固所学的关于 Django 和模板的知识。

- 更新 Rango 应用中所有之前定义的模板，使它们继承新的 `base.html` 模板。按照我们上面演示的相同过程操作。完成后，你的所有模板都应该继承自 `base.html`。
- 同时，确保移除 `index.html` 模板中的链接。我们不再需要它们了！这是因为我们之前已经将它们移到了 `base.html` 中。你也可以移除 `about.html` 模板中指向 Rango 主页的链接。
- 在重构 `index.html` 和 `about.html` 时，保留从静态文件和媒体服务器提供的图片。
- 使用 `url` 模板标签更新所有对 Rango URL 的引用。你也可以在 `views.py` 模块中这么做——查看 [`reverse()` 辅助函数](https://docs.djangoproject.com/en/2.1/ref/urlresolvers/#reverse)。别忘了更新 `<form>` 元素的 `action` 属性中提供的 URL！
- 确保你所有的新模板都在 HTML `<title>` 标签中使用了 `{% block title_block %}`。记住在 `base.html` 中使用 `'Rango - '`，这样所有标题都会带有 `'Rango - '` 前缀。对于继承模板，请使用以下标题。如果你使用每章末尾的测试，这些更改将很重要。
    - 在 `about.html` 中，使用标题 `About Rango`。
    - 在 `add_category.html` 中，使用标题 `Add a Category`。
    - 在 `add_page.html` 中，使用标题 `Add a Page`。
    - 在 `category.html` 中，使用分类的名称——如果提供了不存在的分类，则使用 `Unknown Category`。这里你需要编写一个基本的条件检查（即 `{% if ... %}`）。
    - 最后，在 `index.html` 中，使用标题 `Homepage`。

## 提示

- 首先开始重构 `about.html` 模板。
- 在每个模板中先更新 `title_block`，然后更新 `body_block`。
- 要检查模板中分类是否存在，记得你可以直接使用条件语句 `{% if category %}`——只要 `category` 对象在你的模板上下文中以该名称传递即可！
- 让开发服务器保持运行，并在修改页面时随时检查。不要等到整个页面都改完了才发现它不工作。增量修改并随时测试这些修改是更安全的做法。
- 要引用指向分类页面的链接，你可以使用以下模板代码，特别注意 Django 模板 `{% url %}` 命令。

```
<a href="{% url 'rango:show_category' category.slug %}">
    {{ category.name }}
</a>
```

## 8.4 `render()` 方法和 `request` 上下文

在编写视图时，我们使用了一些不同的方法，其中首选方式是 Django 快捷方法 `render()`。`render()` 方法要求你将 `request` 作为第一个参数传入。`request` 上下文中包含了许多有关会话的信息（例如，包括有关用户的信息）。请参阅 Django 官方文档中关于 Request 对象的内容[^9]。

通过将 `request` 传递给模板，这意味着你在创建模板时也可以访问这些信息。在下一章中，我们将访问与 `user` 相关的信息——但现在，请检查你所有的视图，确保它们都是使用 `render()` 方法实现的。否则，你的模板将无法获得我们后续需要的信息。

[^9]: https://docs.djangoproject.com/en/2.1/ref/request-response/#httprequest-objects

### render() 和 Context

作为你必须执行的检查的一个快速示例，看看 `about()` 视图。最初，它是用硬编码的字符串响应实现的，如下所示。注意我们只发送了字符串——我们没有使用作为 `request` 参数传入的请求。

```
def about(request):
    return HttpResponse('Rango says: Here is the about page. <a href="/rango/">Index</a>')
```

为了使用模板，我们调用 `render()` 函数并传入 `request` 对象。这将允许模板引擎访问诸如请求类型（例如 GET/POST）以及有关用户状态的信息（请参阅用户认证章节）。

```
def about(request):
    # prints out whether the method is a GET or a POST
    print(request.method)
    # prints out the user name, if no one is logged in it prints 'AnonymousUser'
    print(request.user)
    return render(request, 'rango/about.html', {})
```

记住，`render()` 的最后一个参数是上下文字典，你可以用它向 Django 模板引擎传递额外的数据。由于我们没有额外的数据要提供给模板，我们传入一个空字典 `{}`。或者，我们可以完全去掉第三个参数。

## 8.5 自定义模板标签

在每个页面的侧边栏中显示用户可以浏览的不同分类将会很不错。根据我们目前所学到的知识，我们可以这样做：

- 在 `base.html` 模板中，我们可以添加一些代码来显示分类的项目列表；以及
- 在每个视图中，我们可以访问 `Category` 对象，获取所有分类，并在上下文字典中返回它们。

然而，这是一个相当糟糕的解决方案，因为我们需要在所有视图中包含相同的代码。一个 [DRY](https://en.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself)（不要重复自己）的解决方案[^10] 是创建自定义模板标签，将它们包含在模板中，并让它们可以请求*自己的*数据。

### 使用模板标签

创建一个目录 `<workspace>/tango_with_django_project/rango/templatetags`，并在其中创建两个新模块。一个必须命名为 `__init__.py`，并且保持空白。将第二个模块命名为 `rango_template_tags.py`。将以下代码添加到 `rango_template_tags.py` 中。

```python
from django import template
from rango.models import Category

register = template.Library()

@register.inclusion_tag('rango/categories.html')
def get_category_list():
    return {'categories': Category.objects.all()}
```

这段代码中有一个名为 `get_category_list()` 的新方法。该方法返回一个包含一个键/值对的字典。`categories` 表示数据库中存在的所有 `Category` 对象的列表。从上面的 `register.inclusion_tag()` 装饰器中，你会看到引用了 `rango/categories.html`——这是另一个新模板。Django 模板引擎使用这个新模板来渲染你在函数返回的字典中提供的分类列表。然后，这个渲染后的列表可以被注入到最初调用模板标签的视图的响应中！

现在是创建新模板 `categories.html` 的好时机，并添加以下 HTML 标记。请注意这个模板的名称与现有的 `category.html` 模板有相似之处。这是有意设计的，以确保你始终保持警惕！

[^10]: https://en.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself




```python
def get_category_list(current_category=None):
    return {'categories': Category.objects.all(),
            'current_category': current_category}
```

注意 `get_category_list()` 的 `current_category` 参数。如果没有传入参数，则使用 `None`（意味着当前没有选中的类别）。

然后我们可以更新 `base.html` 模板，它使用自定义模板标签来传入当前类别——但仅当该类别存在时。

```html
<div>
    {% block sidebar_block %}
        {% get_category_list category %}
    {% endblock %}
</div>
```

我们现在也可以更新 `categories.html` 模板。这段代码位于现有的 `{% if categories %}` 条件检查内部；它并不会替换整个模板。

```html
{% for c in categories %}
    {% if c == current_category %}
    <li>
    <strong>
        <a href="{% url 'rango:show_category' c.slug %}">{{ c.name }}</a>
    </strong>
    </li>
    {% else %}
    <li>
        <a href="{% url 'rango:show_category' c.slug %}">{{ c.name }}</a>
    </li>
    {% endif %}
{% endfor %}
```

在模板中，我们检查当前显示的类别是否与 `for` 循环中传入的类别相同（即 `c == current_category`）。如果是，我们就通过使用 `<strong>` 标签将类别名称以**粗体**突出显示。
