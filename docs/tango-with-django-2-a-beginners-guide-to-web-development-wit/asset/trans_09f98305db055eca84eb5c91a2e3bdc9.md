
我们将创建一个通用视图，使你能访问 Rango 任何用户的信息。该代码还将允许已登录应用的用户编辑自己的个人资料——*但也仅限于本人的资料*——从而满足[本练习的要求](https://example.org)。

## 创建模板

首先，我们来创建一个用于显示用户个人资料的简单模板。以下 HTML 标记和 Django 模板代码应放置在 Rango 模板目录下新建的 `profile.html` 文件中。

```django
{% extends 'rango/base.html' %}
{% load staticfiles %}

{% block title_block %}
    Profile for {{ selected_user.username }}
{% endblock %}

{% block body_block %}
<div class="jumbotron p-4">
    <div class="container">
        <h1 class="jumbotron-heading">{{ selected_user.username }}'s Profile</h1>
    </div>
</div>

<div class="container">
    <div class="row">
        <img src="{{ MEDIA_URL }}{{ user_profile.picture }}"
             width="300"
             height="300"
             alt="{{ selected_user.username }}'s profile image" />
        <br />
        <div>
            {% if selected_user == user %}
                <form method="post" action="." enctype="multipart/form-data">
                    {% csrf_token %}
                    {{ form.as_p }}

                    <input type="submit" value="Update" />
                </form>
            {% else %}
                <p>
<strong>Website:</strong>
<a href="{{ user_profile.website }}">{{ user_profile.website }}</a>
</p>
{% endif %}
</div>
</div>
</div>
{% endblock %}
```

请注意，在模板中我们使用了几个变量（即 `selected_user`、`user_profile` 和 `form`），需要确保这些变量在模板的上下文中已定义。在此模板中，我们假定：

- `selected_user` 代表一个 Django User 实例，即我们要显示其个人资料的用户；
- `user_profile` 代表 Rango UserProfile 实例，即与我们要显示的关联用户对应的个人资料；
- `form` 代表一个 UserProfileForm 实例，用于显示必要的字段，允许用户编辑自己的个人资料（即网站和/或个人头像），并在必要时预填网站字段。

我们将在下一节中确保该模板的上下文获得所需变量。

此处值得关注的模板块当然是 `body_block` 块。在顶部，我们显示所选用户的个人头像，尺寸设为 300x300 像素。同时，我们还提供了替代文本，以便在无法找到图片时显示。

然而，这个模板的妙处在于其下方的内容。我们使用条件语句来判断当前正在查看某个个人资料的用户是否为该资料所属的用户（`selected_user == user`）——如果是，则显示一个表单，允许该用户编辑自己的资料。如果用户不匹配，我们就不希望提供编辑表单——因此，此时我们只是简单地显示个人资料。

另外请注意，在定义 `<form>` 时，我们再次使用了 `enctype="multipart/form-data"`，因为用户可以上传新的个人头像——请记住，只要涉及文件上传，**必须**指定此属性。

## 创建 `profile()` 视图

基于上面创建的模板，我们可以实现一个简单的视图来处理用户个人资料的查看和表单数据的提交。在 Rango 的 `views.py` 模块中，创建一个名为 `ProfileView` 的新类视图。将如下代码添加到新视图中。

```python
class ProfileView(View):
    def get_user_details(self, username):
        try:
            user = User.objects.get(username=username)
        except User.DoesNotExist:
            return None

        user_profile = UserProfile.objects.get_or_create(user=user)[0]
        form = UserProfileForm({'website': user_profile.website,
                                'picture': user_profile.picture})

        return (user, user_profile, form)

    @method_decorator(login_required)
    def get(self, request, username):
        try:
            (user, user_profile, form) = self.get_user_details(username)
        except TypeError:
            return redirect(reverse('rango:index'))

        context_dict = {'user_profile': user_profile,
                        'selected_user': user,
                        'form': form}

        return render(request, 'rango/profile.html', context_dict)

    @method_decorator(login_required)
    def post(self, request, username):
        try:
            (user, user_profile, form) = self.get_user_details(username)
        except TypeError:
            return redirect(reverse('rango:index'))

        form = UserProfileForm(request.POST, request.FILES, instance=user_profile)

        if form.is_valid():
            form.save(commit=True)
            return redirect('rango:profile', user.username)
        else:
            print(form.errors)

        context_dict = {'user_profile': user_profile,
                        'selected_user': user,
                        'form': form}

        return render(request, 'rango/profile.html', context_dict)
```

我们还需要导入两个此前尚未使用过的类。一个是标准的 Django User 模型，另一个是 Rango 的 UserProfile 模型。请注意，我们之前确实导入过 UserProfileForm——那是不同的东西！

```python
from django.contrib.auth.models import User
from rango.models import UserProfile
```

我们新的类视图要求用户必须登录——因此在对 `get()` 和 `post()` 方法都使用了 `@method_decorator(login_required)` 装饰器。

在 `ProfileView` 类中，我们还创建了一个 `get_user_details()` 辅助方法，以确保代码不重复。该方法首先从数据库中查找 `django.contrib.auth.User` 实例——如果该用户存在的话。如果用户不存在，该方法直接返回 `None`，这向 `get()` 和 `post()` 方法发出信号，让它们简单地重定向到 Rango 的主页，而不是向用户展示一条定制错误消息。毕竟我们无法显示一个不存在用户的信息！如果用户确实存在，`get_user_details()` 方法则会获取 UserProfile 实例（如果不存在，则创建一个空白的实例）。然后，我们获取所选用户的详细信息，并利用这些信息来填充一个 UserProfileForm 实例。最后，用户、user_profile 和已填充的表单对象以元组形式返回，并由调用该辅助方法的 `get()` 和 `post()` 方法解包¹⁰。

在 `post()` 方法中，我们处理用户希望更新其个人资料信息的情况。为此，我们从表单中提取信息到一个 UserProfileForm 实例中，该实例可以引用它正在保存到的 UserProfile 实例——而不是每次都创建一个新的 UserProfile 实例。请记住，我们是*更新*，不是创建！随后保存有效的表单。无效的表单（或 HTTP GET 请求）则会触发 `profile.html` 模板的渲染，并将相关变量通过其上下文传递到模板中。

## 认证练习

我们如何修改上面的代码，以防止未授权用户更改不属于自己的用户账户信息？需要添加什么条件语句来强制执行这一额外检查？

## 将视图映射到 URL

然后，我们需要将新的 `ProfileView` 映射到 URL。这涉及编辑 Rango 的 `urls.py` 模块。我们需要在 urlpatterns 列表中添加一行代码，如下所示。

```
path('profile/<username>/', views.ProfileView.as_view(), name='profile'),
```

注意其中包含一个 `username` 变量——它会匹配 `/profile/` 之后的任何内容。这意味着 URL `/rango/profile/maxwelld90/` 的 username 就是 `maxwelld90`。该值随即作为参数 `username` 传递给类视图中的 `get()` 或 `post()` 方法。正是通过这种方式，我们能够确定所请求的用户是谁。

## 调整基础模板

现在一切应该按预期工作了——但用户如何访问新功能呢？我们可以通过修改 Rango 的 `base.html` 模板来提供一个链接。找到前面在书中创建的构成导航栏的一组 `<li>` 元素。添加一个新链接，允许用户跳转到自己的个人资料——确保该链接*仅在用户登录（已认证）时可见*。如果是这种情况，下面的链接需要放在哪个模板条件语句中？

