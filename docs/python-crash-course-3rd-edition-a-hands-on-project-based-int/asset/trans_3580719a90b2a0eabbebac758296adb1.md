
![](img/8f15563c16d2a98c4d3115150679b092_661_0.png)

图 19-2：*new_entry 页面*

## 编辑条目

现在我们来创建一个页面，以便用户可以编辑他们添加的条目。

### edit_entry 的 URL

该页面的 URL 需要传递要编辑的条目的 ID。下面是 *learning_logs/urls.py*：

**urls.py**

```python
--snip--
urlpatterns = [
    --snip--
    # Page for editing an entry.
    path('edit_entry/<int:entry_id>/', views.edit_entry, name='edit_entry'),
]
```

这个 URL 模式匹配类似 `http://localhost:8000/edit_entry/id/` 的 URL。其中 `id` 的值会被赋给参数 `entry_id`。Django 会将匹配此格式的请求发送给视图函数 `edit_entry()`。

### `edit_entry()` 视图函数

当 `edit_entry` 页面收到 GET 请求时，`edit_entry()` 函数会返回一个用于编辑条目的表单。当页面收到包含修订后条目文本的 POST 请求时，它会将修改后的文本保存到数据库中：

```python
# views.py
from django.shortcuts import render, redirect

from .models import Topic, Entry
from .forms import TopicForm, EntryForm
--snip--

def edit_entry(request, entry_id):
    """Edit an existing entry."""
    entry = Entry.objects.get(id=entry_id)
    topic = entry.topic

    if request.method != 'POST':
        # Initial request; pre-fill form with the current entry.
        form = EntryForm(instance=entry)
    else:
        # POST data submitted; process data.
        form = EntryForm(instance=entry, data=request.POST)
        if form.is_valid():
            form.save()
            return redirect('learning_logs:topic', topic_id=topic.id)

    context = {'entry': entry, 'topic': topic, 'form': form}
    return render(request, 'learning_logs/edit_entry.xhtml', context)
```

我们首先导入 Entry 模型。然后获取用户想要编辑的条目对象①，以及与该条目关联的主题。在 if 代码块（针对 GET 请求执行）中，我们使用参数 instance=entry② 创建 EntryForm 实例。该参数告诉 Django 创建一个表单，并用现有条目对象的信息预填充。用户将看到自己的现有数据，并能够编辑这些数据。

处理 POST 请求时，我们同时传递 instance=entry 和 data=request.POST 这两个参数③。这些参数告诉 Django 基于与现有条目对象关联的信息创建一个表单实例，并使用 request.POST 中的任何相关数据进行更新。然后我们检查表单是否有效；如果有效，我们不带任何参数调用 save()，因为该条目已经与正确的主题关联④。然后我们重定向到主题页面，用户应该能看到他们编辑过的条目的更新版本⑤。

如果我们显示用于编辑条目的初始表单，或者提交的表单无效，我们就创建上下文字典，并使用 edit_entry.xhtml 模板渲染页面。

### edit_entry 模板

接下来，我们创建一个 edit_entry.xhtml 模板，它与 new_entry.xhtml 类似：

**edit_entry.xhtml**

```
{% extends "learning_logs/base.xhtml" %}

{% block content %}
    <p><a href="{% url 'learning_logs:topic' topic.id %}">{{ topic }}</a></p>
    <p>Edit entry:</p>
①  <form action="{% url 'learning_logs:edit_entry' entry.id %}" method='post'>
    {% csrf_token %}
    {{ form.as_div }}
②  <button name="submit">Save changes</button>
</form>

{% endblock content %}
```

`action` 参数把表单发送回 `edit_entry()` 函数进行处理①。我们在 `{% url %}` 标签中加入了 `entry.id` 作为参数，以便视图函数可以修改正确的条目对象。我们将提交按钮标记为 `Save changes`，以提醒用户他们是在保存编辑，而不是在创建新条目②。

### 链接到 edit_entry 页面

现在我们需要在主题页面上为每个条目包含一个指向 `edit_entry` 页面的链接：

**topic.xhtml**

```
--snip--
    {% for entry in entries %}
    <li>
        <p>{{ entry.date_added|date:'M d, Y H:i' }}</p>
        <p>{{ entry.text|linebreaks }}</p>
        <p>
            <a href="{% url 'learning_logs:edit_entry' entry.id %}">
                Edit entry</a></p>
    </li>
--snip--
```

我们在显示每个条目的日期和文本之后包含编辑链接。我们使用 `{% url %}` 模板标签来确定命名 URL 模式 `edit_entry` 的 URL，并结合循环中当前条目的 ID 属性（`entry.id`）。链接文本 `Edit entry` 会出现在页面上的每个条目之后。图 19-3 展示了添加这些链接后的主题页面。

![](img/8f15563c16d2a98c4d3115150679b092_665_0.png)

图 19-3：*每个条目现在都有用于编辑该条目的链接。*

Learning Log 现在已具备其所需的大部分功能。用户可以添加主题和条目，也可以浏览他们想要的任何一组条目。在下一节中，我们将实现一个用户注册系统，以便任何人都可以注册 Learning Log 账户，并创建自己的一组主题和条目。

我们需要添加一行代码，以包含 accounts 应用中的 *urls.py* 文件。这一行将匹配任何以单词 *accounts* 开头的 URL，例如 `http://localhost:8000/accounts/login/`。

## 登录页面

我们将首先实现一个登录页面。我们将使用 Django 提供的默认登录视图，因此这个应用的 URL 模式看起来有些不同。在 `ll_project/accounts/` 目录中新建一个 `urls.py` 文件，并添加以下内容：

**accounts/urls.py**

```python
"""Defines URL patterns for accounts."""

from django.urls import path, include

app_name = 'accounts'
urlpatterns = [
    # Include default auth urls.
    path('', include('django.contrib.auth.urls')),
]
```

我们导入 `path` 函数，然后导入 `include` 函数，以便可以包含一些 Django 已定义的默认身份验证 URL。这些默认 URL 包含命名 URL 模式，例如 'login' 和 'logout'。我们将变量 `app_name` 设置为 'accounts'，以便 Django 能够将这些 URL 与属于其他应用的 URL 区分开来。即使是 Django 提供的默认 URL，当它们被包含在 accounts 应用的 *urls.py* 文件中时，也可以通过 accounts 命名空间访问。

登录页面的模式匹配 URL `http://localhost:8000/accounts/login/`。当 Django 读取这个 URL 时，单词 *accounts* 告诉 Django 在 *accounts/urls.py* 中查找，而 *login* 则告诉它将请求发送到 Django 的默认 *login* 视图。

### 登录模板

当用户请求登录页面时，Django 将使用默认的视图函数，但我们仍然需要为该页面提供一个模板。默认的身份验证视图会在名为 *registration* 的文件夹中查找模板，因此我们需要创建这个文件夹。在 *ll_project/accounts/* 目录中，创建一个名为 *templates* 的目录；在其中，再创建另一个名为 *registration* 的目录。下面是 *login.xhtml* 模板，它应保存在 *ll_project/accounts/templates/registration* 中：

**login.xhtml**

```
{% extends 'learning_logs/base.xhtml' %}

{% block content %}

①   {% if form.errors %}
        <p>Your username and password didn't match. Please try again.</p>
    {% endif %}

②   <form action="{% url 'accounts:login' %}" method='post'>
        {% csrf_token %}
③      {{ form.as_div }}

④      <button name="submit">Log in</button>
    </form>

{% endblock content %}
```

该模板继承自 *base.xhtml*，以确保登录页面与网站其他部分具有相同的外观和感觉。请注意，一个应用中的模板可以继承另一个应用中的模板。

如果表单的 *errors* 属性已设置，我们会显示一条错误消息①，报告用户名和密码组合与数据库中存储的任何内容都不匹配。

我们希望登录视图处理该表单，因此我们将 `action` 参数设置为登录页面的 URL②。登录视图会向模板发送一个 `form` 对象，我们需要自己来显示表单③并添加提交按钮④。

### LOGIN_REDIRECT_URL 设置

用户成功登录后，Django 需要知道将该用户重定向到哪里。我们在设置文件中控制这一点。

在 `settings.py` 的末尾添加以下代码：

```python
# --snip--
# My settings.
LOGIN_REDIRECT_URL = 'learning_logs:index'
```

由于 `settings.py` 中包含所有默认设置，因此标注出我们正在添加新设置的部分会很有帮助。我们要添加的第一个新设置是 `LOGIN_REDIRECT_URL`，它告诉 Django 在登录尝试成功后应重定向到哪个 URL。

### 链接到登录页面

让我们将登录链接添加到 `base.xhtml` 中，以便它出现在每个页面上。我们不希望在用户已经登录时显示该链接，因此我们将其嵌套在 `{% if %}` 标签内：

```html
<p>
    <a href="{% url 'learning_logs:index' %}">Learning Log</a>
    - <a href="{% url 'learning_logs:topics' %}">Topics</a> -
    {% if user.is_authenticated %}
        Hello, {{ user.username }}.
    {% else %}
        <a href="{% url 'accounts:login' %}">Log in</a>
    {% endif %}
</p>
```
