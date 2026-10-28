
由 Feather²（https://feathericons.com/）提供，并作为 Bootstrap 框架的一部分被包含在内。另请注意，`<strong>` 和 `<button>` 元素都包含了 `id` 属性。我们赋予的 `id` 值非常重要，因为我们的 jQuery 代码会用到它们。

![](img/75169a27287f7c945bf2cbf7dede5251_307_0.png)

更新 `category.html` 模板后的结果。注意“点赞分类”按钮，其旁边有点赞数——所有这些都位于分类标题的正下方。如果你在未登录状态下查看同一页面，该按钮将不可见。

## 创建点赞分类视图

现在，既然已经在 Rango 的 `category.html` 中打下了基础，让我们把注意力转向服务器端，集中实现处理点赞分类请求的视图。回想一下，在我们的概述中，我们提到将使用 GET 请求来点赞分类。这些请求将通过形如 `/rango/like_category/?category_id=1` 的 URL 来服务。根据这两个需求，我们的基于类的视图只需要实现 `get()` 方法，该方法从 GET 对象中提取 `category_id` 查询字符串。

我们的实现如下所示，将位于 Rango 的 `views.py` 模块中。

```python
class LikeCategoryView(View):
    @method_decorator(login_required)
    def get(self, request):
        category_id = request.GET['category_id']

        try:
            category = Category.objects.get(id=int(category_id))
        except Category.DoesNotExist:
            return HttpResponse(-1)
        except ValueError:
            return HttpResponse(-1)

        category.likes = category.likes + 1
        category.save()

        return HttpResponse(category.likes)
```

检查上面的代码，你可以看到我们只允许已登录用户访问此视图——因此使用了 `@method_decorator(login_required)` 装饰器。我们还实现了一些基本的错误处理。如果用户提供的分类 ID 不存在，或者提供的分类 ID 无法转换为整数，则会在响应中返回 -1。否则，给定分类的 likes 属性会增加 1。更新后的值随后会被单独返回。

你应该已经拥有了所有必要的 import 语句——但请再次确认以下内容存在。这个语句在本书开头就使用过——你在前几章清理代码时可能把它删掉了。你现在绝对需要它！

```python
from django.http import HttpResponse
```

当然，如果没有对应的 URL 映射，这个视图将毫无用处。为了满足需求，让我们添加一个映射。将以下内容添加到 Rango 的 `urls.py` 模块的 `urlpatterns` 列表中。

```python
path('like_category/', views.LikeCategoryView.as_view(), name='like_category'),
```

现在，你应该可以进入下一步了——发出 AJAX 请求。

## 发出 AJAX 请求

为了实现 AJAX 功能，请打开位于项目 `static` 目录下的空白 `rango-ajax.js` 文件，并添加以下 JavaScript 代码。

```javascript
$(document).ready(function() {
    $('#like_btn').click(function() {
        var categoryIdVar;
        categoryIdVar = $(this).attr('data-categoryid');

        $.get('/rango/like_category/',
            { 'category_id': categoryIdVar},
            function(data) {
                $('#like_count').html(data);
                $('#like_btn').hide();
            }
        );
    });
});
```

这段 JavaScript/jQuery 代码会在页面加载完成后触发，然后将代码绑定到“点赞分类”按钮（通过唯一标识符 `like_btn` 识别）的事件处理程序上。当用户点击此按钮时，`category_id` 会从按钮的 `data-categoryid` 属性中提取。如果你仔细查看本章前面定义的模板代码，`data-categoryid` 属性是由 Django 模板引擎在服务器端渲染页面时，用分类的唯一 ID 填充的！一旦获得 `category_id`，我们就调用 `$.get()`。

`$.get()` 是一个 jQuery 函数，用于处理 AJAX GET 请求。我们首先指定要访问的 URL（在此实例中是硬编码的，这并不理想！），然后传入一个类似字典的对象作为第二个参数。根据提供的键/值对，会构造出一个查询字符串。这意味着 jQuery 构建的最终请求将类似于 `GET /rango/like_category/?category_id=<category_id_var>`。实际的 ID 值当然会被插入到 URL 末尾的相应位置。

我们提供的最后一个参数是另一个匿名函数，这次它接收一个 `data` 参数。当服务器对请求作出响应时，该函数会被调用，`data` 包含服务器的响应。在我们的例子中，其中将包含该分类的点赞数。

![](img/75169a27287f7c945bf2cbf7dede5251_311_0.png)

Django 开发服务器的输出，显示了正在进行的 AJAX 请求（以红色突出显示）。该请求显示传递的 `category_id` 被设置为 1——从上面的日志（显示初始分类页面加载）可以看出，该请求是针对 Python 分类的。

## 17.3 添加内联分类建议

在实现了一个简单的 AJAX 示例之后，让我们尝试一些更复杂的东西。假设 Rango 中已经填充了几十个分类。如果用户想查找一个特定的分类，他们目前需要浏览一长串分类。这太烦人了！

要让用户更快地找到他们想要的分类，一种方法是提供一个建议组件。用户可以开始输入他们正在搜索的分类名称，Rango 会返回一个建议分类列表，供用户选择。本质上，此功能将提供一种*过滤*形式，让用户能够缩小可用的选项范围。

当然，我们可以通过 AJAX 实现这一点。每当用户从键盘输入一个字母时，我们就可以向 Django 服务器发送一个请求，要求它返回一个潜在过滤后的分类匹配列表。然后，这个分类列表可以在已加载的页面中渲染。为了确保这一功能在 Rango 应用中都能使用，我们将把它实现在左侧的分类列表部分。

### 工作流程

要实现这一点，我们需要执行以下步骤。

1. 首先，我们需要创建一个带参数的函数 `get_category_list()`。此函数将接受两个参数，即 `max_results`（默认为 0）和 `starts_with`（默认为空字符串 `''`）。该函数将返回一个匹配用户查找条件的分类列表。
   - `max_results` 将限制返回的结果数量。如果指定为 0，则对返回多少分类不设限制。
   - `starts_with` 表示用户到目前为止输入的内容。想象一下，如果用户正在查找 `python`，那么这里可能传入的字符串是 `pyt`——意味着用户已经输入了他们要查找内容的前三个字符。
2. 还需要创建一个进一步的视图。我们将其命名为 `CategorySuggestionView`。这同样将是基于类的视图，并通过 URL 映射 `/rango/suggest/`（名称为 `suggest`）进行访问。该视图将接收用户的建议，并返回用户输入内容的前八条建议列表。
   - 我们假设这是通过 HTTP GET 请求实现的。
   - 我们将检查提供给视图的查询字符串是否为空。如果其中包含内容，我们将调用 `get_category_list()` 来获取所提供字符串的前八个结果。
   - 来自 `get_category_list()` 的结果将在现有模板 `categories.html` 中组合呈现。我们之前就是通过自定义模板标签，用这个模板来显示 Rango 页面左侧的分类列表。

