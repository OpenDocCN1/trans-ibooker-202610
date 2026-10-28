

```python
urlpatterns = [
    ...

    # Updated path that points to the new about class-based view.
    path('about/', views.AboutView.as_view(), name='about')

    ...
]
```

由于我们现有的导入语句只是导入了 Rango 的 `views.py`，因此我们必须指定 `views.AboutView`，以精确告诉 Python 新类的位置。当然，你始终可以单独导入 `AboutView`，如下面的示例所示。

```python
from rango.views import AboutView

urlpatterns = [
    ...

    # Updated path that points to the new about class-based view.
    path('about/', AboutView.as_view(), name='about')

    ...
]
```

注意，我们还调用了 `as_view()` 函数。这是基础 `View` 类的一部分，它提供了必要的代码，让 Django 能够使用你在 `get()` 方法中定义的逻辑来处理视图。

对于这样一个简单的视图，你可能会觉得改用基于类的方法并不会节省多少时间或空间。不过，现在你应该已经明白其中的思路了——而且你应该能够开始重构更复杂的视图，使其使用基于类的方法。

一个同时考虑 HTTP GET 和 HTTP POST 的较复杂视图可以是 `add_category()` 视图。要将其从基于函数的视图转换为基于类的视图，我们可以在 Rango 的 `views.py` 中创建一个新的 `AddCategoryView` 类。然后，我们就可以开始将代码从函数迁移到新类中，如下所示。仔细看看 `get()` 和 `post()` 方法中的代码——它们来自哪里呢？

```python
class AddCategoryView(View):
    @method_decorator(login_required)
    def get(self, request):
        form = CategoryForm()
        return render(request, 'rango/add_category.html', {'form': form})

    @method_decorator(login_required)
    def post(self, request):
        form = CategoryForm(request.POST)

        if form.is_valid():
            form.save(commit=True)
            return redirect(reverse('rango:index'))
        else:
            print(form.errors)

        return render(request, 'rango/add_category.html', {'form': form})
```

将你目前在 `add_category()` 函数式视图中编写的代码与上面的代码进行比较——HTTP `get()` 请求的逻辑镜像到了 `get()` 方法中，HTTP POST 请求的逻辑同样镜像到了 `post()` 方法中。你还会注意到，我们使用了一个名为 `method_decorator` 的新装饰器函数。这需要导入，因此请在 `views.py` 模块顶部添加以下导入语句。

```python
from django.utils.decorators import method_decorator
```

通过将 `@method_decorator(login_required)` 同时关联到 `get()` 和 `post()` 方法，我们可以确保未登录 Rango 的用户无法访问这两个方法。如果我们从 `get()` 方法中移除装饰器，你觉得会发生什么？用户将看到添加新类别的表单，但由于 `post()` 方法仍然受保护，未登录的用户将无法创建新类别！

## 为什么需要 @method_decorator？

我们需要 `@method_decorator`，因为函数装饰器需要被转换为方法装饰器⁶——`method_decorator()` 可以为我们实现这一点。

> ⁶ https://docs.djangoproject.com/en/2.2/topics/class-based-views/intro/#decorating-the-class

最后一件事——我们还需要更新 Rango 的 `urls.py` 模块，以反映我们现在要使用基于类的视图这一事实。在 `urlpatterns` 列表中，找到 `add_category` 映射，并将该条目修改为如下示例所示。

```python
path('add_category/', views.AddCategoryView.as_view(), name='add_category'),
```

仔细观察这个示例，我们只是更改了目标视图。我们不再指向 `add_category()` 视图，而是指向新的 `AddCategoryView` 类——并调用 Django 提供的 `as_view()` 方法，该方法会根据收到的请求将控制权传递给我们相应的某个方法（我是要执行 `get()` 还是 `post()` 呢？）。

## 类视图练习

既然你已经了解了在 Django 中实现类视图的基础知识，现在正是动手实践的好时机。

- 查看 [Django 文档](https://docs.djangoproject.com/en/2.1/topics/class-based-views/)，寻找更多关于如何创建类视图的示例。
- 更新你的 Rango 应用，确保 Rango 的 `views.py` 模块中的*所有*视图都使用类视图！
  - 有一个节省时间的提示：请记住，你一直在使用的两个 `urls.py` 模块都引用了当前的 `index()` 视图！当你实现一个基于类的替代方案时，务必记住这一点。

这不会像你想的那么耗时。最难的部分在于那些同时具有 HTTP GET 和 HTTP POST 功能的较复杂视图。能够区分每种请求类型的流程，并将必要的代码放入不同的方法中，这将检验你对代码的理解！

在你完成这个练习时，我们还有几点希望你思考一下。我们将其作为单独的小贴士列在下面。

### 命名约定

确保你的新类视图符合 [Python 命名约定](https://www.python.org/dev/peps/pep-0008/)是一种良好的实践。对于类，请使用 CamelCase（驼峰命名法）。将类名的开头大写，然后每个后续单词的首字母也大写——单词之间不留空格。因此，新的添加类别视图变为 `AddCategoryView`。在末尾加上 `View`，以确保以后查看你代码的人知道他们正在处理的是一个基于类的视图。

类中的方法应遵循与命名函数相同的约定。使用小写名称，并用下划线分隔各个单词——例如 `some_method_name()`。

### 映射 IndexView

当你为旧的 `index()` 视图创建基于类的视图时，请注意你已经在 Rango 的 `urls.py` 模块*以及*项目的 `urls.py` 模块中对其进行了映射！

### 传递 URL 参数

之前你创建了一个 `show_category()` 视图。除了必需的 `request` 对象之外，它还接受一个 `category_name_slug`——这是一个*参数化视图*，参数来自 URL。对于所有以这种方式实现的视图，你需要确保它们基于类的等效视图不仅包含 `self` 和 `request` 作为参数，还要包含一个或多个 URL 参数。在上面的示例中，`get()` 方法的定义可能如下所示。

```python
class ShowCategoryView(View):
    def get(self, request, category_name_slug):
        ...
```

如果你要处理同时具有 `get()` 和 `post()` 方法的视图，那么这两个方法都需要这个（或这些）参数！

### 🔑 类和辅助方法

你可能会发现，在实现基于类的视图时，你会重复自己的代码。例如，在基于函数的 `show_category()` 视图中，我们有一些相当冗长的代码，用于填充一个字典，作为传递给 `render()` 的上下文字典。这段代码对于 HTTP GET 和 HTTP POST 请求都会执行。在基于类的实现中，一种解决方案是将这些代码简单地添加到 `get()` 和 `post()` 方法中……但请记住，[不要重复自己（DRY）](https://en.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself)！你如何设计一种解决方案，使得只需要一份生成上下文字典的代码实例？

一个可能的解决方案是在你的基于类的实现中创建一个*辅助方法*。你可以随意命名——但要成功为 `show_category()` 视图生成上下文字典的基础内容，你还需要 `category_name_slug`。你该如何实现这一点？我们在上面的提示中给了你一个暗示。一旦你弄明白了，你还可以将同样的技术应用到你代码中的其他类视图上。

## 15.5 查看你的个人资料

现在，恢复使用 `UserProfile` 模型的代码已经就位，我们可以实现功能，允许用户查看自己的个人资料——并进行编辑。这个过程同样与我们之前多次完成的操作类似。我们需要考虑以下三步。

1. 首先，我们需要创建一个新模板，这次名为 `profile.html`。它将位于 `rango` 模板目录中。
2. 我们必须创建一个新的视图来渲染这个新模板。我们这次从一开始就将其作为基于类的视图来创建！
3. 最后，新视图必须映射到一个 URL——在本例中，即 `/rango/profile`。

当然，为了让新页面可访问，我们还需要在 Rango 的 `base.html` 模板中提供一个额外的超链接，以便轻松访问。对于这个解决方案，

