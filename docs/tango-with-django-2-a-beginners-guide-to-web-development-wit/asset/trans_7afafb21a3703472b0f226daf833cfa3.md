

## Django 用户注册应用

值得注意的是，市面上有一些现成的用户注册应用，能够减少很多自行构建注册和登录表单的麻烦。

然而，在使用这些应用之前，最好先对底层机制有所了解。这能确保你对幕后发生的一切有一定感知。*一分耕耘，一分收获。* 这也会加深你对表单处理、如何扩展 User 模型以及如何上传媒体文件的理解。

为了提供用户注册功能，我们现在将逐步完成以下四个步骤：

- 创建 `UserForm` 和 `UserProfileForm`；
- 添加一个视图来处理新用户的创建；
- 创建一个显示 `UserForm` 和 `UserProfileForm` 的模板；以及
- 将 URL 映射到所创建的视图。

作为整合新注册功能的最后一步，我们还将：

- 将首页链接到注册页面。

## 创建 UserForm 和 UserProfileForm

在 `rango/forms.py` 中，我们需要创建两个继承自 `forms.ModelForm` 的类。我们将为基本的 User 类创建一个，也为刚创建的新 UserProfile 模型创建一个。这两个继承自 `ModelForm` 的类允许我们显示一个 HTML 表单，展示特定模型所需的表单字段，从而为我们省去大量工作。

让我们在 `rango/forms.py` 中创建这两个继承自 `forms.ModelForm` 的类。请将以下代码添加到模块中。

```
# until we're ready to avoid integrity problems.
profile = profile_form.save(commit=False)
profile.user = user

# Did the user provide a profile picture?
# If so, we need to get it from the input form and
#put it in the UserProfile model.
if 'picture' in request.FILES:
    profile.picture = request.FILES['picture']

# Now we save the UserProfile model instance.
profile.save()

# Update our variable to indicate that the template
# registration was successful.
registered = True
else:
    # Invalid form or forms - mistakes or something else?
    # Print problems to the terminal.
    print(user_form.errors, profile_form.errors)
else:
    # Not a HTTP POST, so we render our form using two ModelForm instances.
    # These forms will be blank, ready for user input.
    user_form = UserForm()
    profile_form = UserProfileForm()

# Render the template depending on the context.
return render(request,
    'rango/register.html',
    context = {'user_form': user_form,
               'profile_form': profile_form,
               'registered': registered})
```

虽然这个视图看起来相当复杂，但它实际上与我们实现 add_category 和 add_page 视图的方式非常相似。不过，这里的关键区别在于我们必须处理两个不同的 ModelForm 实例——一个用于 User 模型，一个用于 UserProfile 模型。此外，如果用户选择上传个人资料图片，我们还需要处理它。

我们在这里使用的一个技巧（过去曾让很多人栽过跟头）是在保存 UserProfile 表单时使用 `commit=False`。这会让 Django 在第一次保存时不会将数据写入数据库。为什么要这样做？请记住，来自 `UserProfileForm` 表单的信息会被传递到 `UserProfile` 模型的新实例上。`UserProfile` 包含一个对标准 Django `User` 模型的外键引用——但 `UserProfile` 本身并不提供这个信息！试图以不完整的状态保存新的实例会引发引用完整性错误。*两个模型之间的关联是必需的。*

为了解决这个问题，我们让 `UserProfileForm` *不要*立即保存，如上文第 29 行所示。这样我们就可以通过第 30 行的 `profile.user=user` 来添加 `User` 引用。完成后，我们可以在第 39 行调用 `profile.save()` 方法，手动将新实例保存到数据库。此时引用完整性得到保证，一切都会按预期工作。

## 创建*注册*模板

现在我们需要创建新 `register()` 视图将要使用的模板。请创建一个新的模板文件 `rango/register.html`，并添加以下代码。

```
1 {% extends 'rango/base.html' %}
2 {% load staticfiles %}
3 
4 {% block title_block %}
5     Register
6 {% endblock %}
7 
8 {% block body_block %}
9 <h1>Register for Rango</h1>
10 {% if registered %}
11 Rango says: <strong>thank you for registering!</strong>
12 <a href="{% url 'rango:index' %}">Return to the homepage.</a><br />
13 {% else %}
14 Rango says: <strong>register here!</strong><br />
15 <form id="user_form" method="post" action="{% url 'rango:register' %}"
16     enctype="multipart/form-data">
17 
18     {% csrf_token %}
19 
20     <!-- Display each form  -->
21    {{ user_form.as_p }}
22    {{ profile_form.as_p }}
23
24    <!-- Provide a button to click to submit the form. -->
25    <input type="submit" name="submit" value="Register" />
26 </form>
27 {% endif %}
28 {% endblock %}
```

## 使用 url 模板标签

请注意，在上述模板代码中我们使用了 url 模板标签，例如 {% url 'rango:register' %}。这意味着我们在映射 URL 时必须将其命名为 register。

这里需要注意的第一点是，该模板使用了我们在视图中用来表示注册是否成功的 registered 变量。请注意，模板必须将 registered 设为 False 才会显示注册表单——否则会显示成功消息。

接下来，我们对 user_form 和 profile_form 使用了 as_p 模板方法。这会将表单中的每个元素包裹在一个段落中（由 <p> HTML 标签表示）。这样可以确保每个元素都出现在新的一行上。

最后，在 <form> 元素中，我们包含了 enctype 属性。这是因为如果用户尝试上传图片，表单的响应可能包含二进制数据。这与从用户输入表单字段中得到的文本数据不同。因此，响应必须分成多个部分才能传回服务器。因此，我们需要用 enctype="multipart/form-data" 来标明这一点。这告诉 HTTP 客户端（Web 浏览器）相应地打包并发送数据。否则，服务器将无法收到用户提交的所有数据。

## 多部分消息与二进制文件

你应该了解 <form> 元素的 enctype 属性。当你希望用户通过表单上传文件时——无论是图片还是其他文档——绝对必须将 enctype 设置为 multipart/form-data。这种属性和值的组合会指示浏览器以特殊方式将表单数据发送回服务器。本质上，代表文件的数据会被拆分成一系列数据块并发送。有关更多信息，请查看这个很棒的 Stack Overflow 回答¹³。

此外，请务必在 <form> 元素中包含 CSRF 令牌，即 {% csrf_token %}！如果你不这样做，Django 的跨站伪造¹⁴ 保护中间件层将拒绝接受表单内容并返回错误。这提供了一层安全保护，防止来自伪造网站的提交通信成功。这通常被称为网络钓鱼。

## register() URL 映射

创建好新的视图和关联模板后，我们现在可以添加 URL 映射。在 Rango 的 URLs 模块 rango/urls.py 中，按如下所示修改 urlpatterns 元组。

```
urlpatterns = [
    path('', views.index, name='index'),
    path('about/', views.about, name='about'),
    path('category/<slug:category_name_slug>/add_page/', views.add_page,
        name='add_page'),
    path('category/<slug:category_name_slug>/', views.show_category,
        name='show_category'),
    path('add_category/', views.add_category, name='add_category'),
    path('register/', views.register, name='register'),  # New mapping!
]
```

新添加的模式（位于列表底部）将 URL /rango/register/ 指向 register() 视图。另请注意，我们为新的 URL 指定了名称 register。

