## 在 Django 中测试

本章提供了 Django 测试的基础知识，结构类似于 [Django 教程](https://docs.djangoproject.com/en/2.1/intro/tutorial05/)——并附带一些额外说明。如果有读者强烈希望我们进一步扩展本章，我们会！如果你认为这对你有帮助，请与我们联系。

### 18.1 运行测试

Django 附带了一整套工具来测试你所构建的应用。你可以使用终端或命令提示符来测试你开发的 Rango 应用。只需发出以下命令，并观察输出。

```text
$ python manage.py test rango

Creating test database for alias 'default'...

-----------------------------------------------------------------
Ran 0 tests in 0.000s

OK
Destroying test database for alias 'default'...
```

此命令将运行为 Rango 应用创建的所有测试。然而，目前什么也没有发生。这是因为 rango 应用目录中的 `tests.py` 几乎是空白的，只有一条导入语句。每次你创建新的 Django 应用时，都会为你创建这个近乎空白的 `tests.py` 模块，以鼓励你编写测试！

从上面显示的输出中，你可能还会注意到引用了一个名为 `default` 的数据库。当你运行测试时，会构建一个临时数据库，你的测试可以在其中填充数据并进行操作。这样，你的测试就*独立于*你的实际数据库运行，满足了我们上面列出的规则之一。

### 测试 Rango 的模型

鉴于目前没有测试，让我们创建一个！在 `Category` 模型中，我们想要确保接收的浏览量大于或等于零，因为你不能有负的浏览量。为此，我们可以将以下代码放入 Rango 的 `tests.py` 模块中，注意保留现有的 `import` 语句。

```python
class CategoryMethodTests(TestCase):
    def test_ensure_views_are_positive(self):
        """
        Ensures the number of views received for a Category are positive or zero.
        """
        category = Category(name='test', views=-1, likes=0)
        category.save()

        self.assertEqual((category.views >= 0), True)
```

你还需要确保导入 `Category` 模型。

```python
from rango.models import Category
```

关于我们编写的测试，首先要注意的是，我们必须将测试放在一个类中。这个类必须继承自 `django.test.TestCase`。类中实现的每个方法测试一个特定功能。这些方法的名字应始终以 `test_` 开头，并且应始终包含某种断言。在本例中，我们使用 `assertEqual` 来检查两个项是否相等。但是，你还可以使用许多其他断言检查——如[官方 Python 文档](https://docs.python.org/3/library/unittest.html#assert-methods)所示。Django 的测试机制⁴

⁴https://docs.python.org/3/library/unittest.html#assert-methods


源自Python实现，但也提供了几个Django和Web开发特有的断言和测试用例。

如果我们随后运行测试，将会看到以下输出。

```text
$ python manage.py test rango

Creating test database for alias 'default'...
F
======================================================================
FAIL: test_ensure_views_are_positive (rango.tests.CategoryMethodTests)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/Users/maxwellld90/Workspace/tango_with_django_project/rango/tests.py", line 12, in test_ensure_views_are_positive
    self.assertEqual((cat.views>=0), True)
AssertionError: False != True

----------------------------------------------------------------------
Ran 1 test in 0.001s

FAILED (failures=1)
```

我们可以看到Django识别到了我们唯一的测试，并且它失败了。这是因为模型没有检查`views`的值是否小于零。既然我们希望确保该字段的值非零，我们需要更新模型以确保满足这一要求。现在更新模型，在Rango的`models.py`模块中的`Category`模型的`save()`方法里添加一些代码。代码应该检查`views`属性的值，如果提供的值小于零，则将其设置为零。对`self.views`进行简单的条件检查就足够了。

更新模型后，重新运行测试。看看你的代码现在是否通过了测试。如果没有，再试一次，找出一个能通过测试的解决方案。

我们尝试添加另一个测试，确保创建了适当的slug。这意味着slug用破折号代替空格（`-`），并且全部为小写。将以下测试方法添加到你的新`CategoryMethodTests`类中。

```python
class IndexViewTests(TestCase):
    def test_index_view_with_no_categories(self):
        """
        If no categories exist, the appropriate message should be displayed.
        """

        response = self.client.get(reverse('rango:index'))

        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'There are no categories present.')
        self.assertQuerysetEqual(response.context['categories'], [])
```

由于我们使用Django的`reverse()`函数来执行URL查找，我们还需要确保在`tests.py`模块顶部包含正确的导入语句。

```python
from django.urls import reverse
```

查看上面的代码，Django的`TestCase`类可以访问一个能够发出请求的`client`对象。这里，它使用辅助函数`reverse()`来查找Rango索引页面的URL。然后尝试对该页面发出HTTP GET请求。响应被返回并存储在`response`中。然后测试会检查几件事：页面是否成功加载（返回200状态码）；响应的HTML是否包含字符串“There are no categories present.”；以及用于渲染响应的上下文字典中，所提供的categories是否是一个空列表。

回忆一下，当运行测试时，会创建一个新的数据库，默认情况下该数据库是空的。这对每个测试方法都是如此——这解释了为什么你在两个`CategoryMethodTests`测试中创建的类别对`IndexViewTests`中的测试不可见。

现在让我们检查存在类别时的索引视图。我们可以添加一个辅助函数来实现这一点。这个简单的函数不在类内部——只需将其放在`tests.py`模块中。

运行测试。它能通过吗？

## 更新你现有的测试

更新你目前已经创建的现有测试，以使用辅助函数`add_category()`。

## 测试渲染后的页面

Django的测试套件还允许你执行加载Web应用并程序化地与渲染后HTML页面上的DOM元素进行交互的测试。这非常有用，因为你可以像人类一样测试Web应用——通过‘点击’链接，或在表单字段中输入信息并提交。这需要借助一些额外的第三方“驱动程序”软件，它们为你控制网页上的交互。

我们在这里没有明确介绍如何设置这些测试，但你应该参考[官方Django文档](https://docs.djangoproject.com/en/2.2/topics/testing/tools/#liveservertestcase)来了解更多并了解如何实现。

如果你真的希望我们提供如何以这种方式运行测试的示例，请告诉我们！

## 18.2 检查测试覆盖率

关于测试的另一个讨论点是*覆盖率*。代码覆盖率衡量你的代码库中有多少已经被测试。你可以安装一个名为coverage的额外包（`$ pip install coverage`），它可以自动分析你的代码库中有多少已被测试覆盖。安装coverage后，在终端或命令提示符中运行以下命令。

```bash
$ coverage run --source='.' manage.py test rango
```

这将运行你到目前为止实现的所有测试，并为Rango应用收集覆盖率数据。要查看报告，你需要在第一条命令完成后输入以下命令。

```text
$ coverage report
Name                                      Stmts   Miss  Cover
-------------------------------------------------------------
manage.py                                     9      2    78%
populate_rango.py                            33     33     0%
rango/__init__.py                              0      0   100%
rango/admin.py                                10      0   100%
rango/apps.py                                  3      3     0%
rango/bing_search.py                          38     32    16%
rango/forms.py                                34      6    82%
rango/migrations/0001_initial.py               6      0   100%
rango/migrations/0002_auto_20200111_1313.py     4      0   100%
rango/migrations/0003_category_slug.py          4      0   100%
rango/migrations/0004_auto_20200111_1702.py     6      0   100%
rango/migrations/__init__.py                    0      0   100%
rango/models.py                                33      3    91%
rango/templatetags/__init__.py                  0      0   100%
rango/templatetags/rango_template_tags.py       6      0   100%
rango/tests.py                                 34      0   100%
rango/urls.py                                   4      0   100%
rango/views.py                                207    139    33%
tango_with_django_project/__init__.py           0      0   100%
tango_with_django_project/settings.py          28      0   100%
tango_with_django_project/urls.py              12      1    92%
tango_with_django_project/wsgi.py               4      4     0%
-------------------------------------------------------------
TOTAL                                         475    223    53%
```

从该命令的输出中，我们可以看到代码的关键部分尚未被测试。例如，`views.py`的覆盖率相当低，仅34%。因此，这个输出可以为你提供一种基于度量的方法，以确定应将测试编写工作重点放在哪里。

coverage包[还有更多功能](https://coverage.readthedocs.io/en/latest/)，你可以探索这些功能，使你的测试更加全面！

### 提示

你需要使用Django的时区感知功能来获取当前日期和时间。这需要你导入：`from django.utils import timezone`，通过`timezone.now()`来访问当前日期和时间。

为了比较两个日期，你需要使用`assertTrue()`方法进行断言。该方法接受一个布尔表达式，你可以在其中执行评估，例如`page.last_visit < timezone.now()`。

在进行第二个测试时，你需要调用`rango:goto`视图，并将`page_id`作为参数传递。你可以使用`client.get()`方法，通过提供一个字典作为第二个参数来实现，其中键为`page_id`，值为你希望访问的页面的ID。根据你实现该测试的方式，可能还需要从数据库刷新模型实例。这可以使用`refresh_from_db()`方法来完成。

作为最后的提示，编写一个额外的辅助函数来向给定类别添加页面也可能有所帮助。如果遇到困难，我们已经在GitHub上实现了一个[模型解决方案](https://github.com/maxwelld90/tango_with_django_2_code/tree/ce0911025891970d6ee3ac5b8745ea970477151c/)。

### 其他测试资源

- 浏览[官方Django教程第五部分](https://docs.djangoproject.com/en/2.1/intro/tutorial05/)以了解更多关于测试的内容。
- 查看[Harry Percival关于测试驱动开发的教程](https://www.obeythetestinggoat.com/book/part1.harry.html)。



