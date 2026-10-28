

```text
$ python manage.py migrate

Operations to perform:
        Apply all migrations: admin, auth, contenttypes, rango, sessions
Running migrations:
        Applying rango.0001_initial... OK
```

此输出确认数据库表已在你的数据库中创建完成，随后你便可以开始使用新的模型和表了。

然而，你可能已经注意到，我们的 `Category` 模型目前还缺少 Rango 需求中规定的一些字段。**别担心，这些字段稍后会被添加，届时你将再次经历一次迁移过程。**

## 5.5 Django 模型与 Shell

在我们转向演示 Django 管理界面之前，值得注意的是，你可以直接在 Django Shell 中与 Django 模型交互——这是一个非常有用的调试工具。我们将演示如何使用这种方法创建一个 Category 实例。

要进入 Shell，我们需要再次在 Django 项目的根目录下调用 `manage.py`。运行以下命令。

```text
$ python manage.py shell
```

这会启动一个 Python 解释器实例，并为你加载项目的设置。然后你就可以与模型交互了，下面的终端会话演示了这一功能。请查看我们添加的内联注释，了解每条命令的作用。

```text
# Import the Category model from the Rango application
>>> from rango.models import Category

# Show all the current categories
>>> print(Category.objects.all())
# Since no categories have been defined we get an empty QuerySet object.
<QuerySet []>

# Create a new category object, and save it to the database.
>>> c = Category(name='Test')
>>> c.save()

# Now list all the category objects stored once more.
>>> print(Category.objects.all())
# You'll now see a 'Test' category.
<QuerySet [<Category: Test>]>

# Quit the Django shell.
>>> quit()
```

在示例中，我们首先导入要操作的模型。然后打印出所有现有的类别。由于底层的 Category 表是空的，因此返回了一个空列表。接着我们创建并保存一个 Category，然后再次打印出所有类别。第二次打印显示了刚刚添加的新 Category。注意，第二次打印中出现了名字 Test——这是 `__str__()` 的输出，它很好地说明了为什么包含这些方法很重要！

### 完成官方教程

上面的示例只是让你初步体验了在 Django Shell 中可以执行的数据库相关操作。如果你还没有完成，现在是完成[官方 Django 教程第二部分](https://docs.djangoproject.com/en/2.1/intro/tutorial02/)的好时机，以了解更多关于模型交互的知识。此外，还可以查看[官方 Django 文档中关于可用命令列表](https://docs.djangoproject.com/en/2.1/ref/django-admin/#available-commands)的部分，用于处理模型。

![](img/75169a27287f7c945bf2cbf7dede5251_84_0.png)

Django 管理界面，未包含 Rango 模型。

虽然看起来不错，但我们缺少为 Rango 应用定义的 Category 和 Page 模型。要包含这些模型，我们需要告知 Django 我们想包含它们。

为此，请打开文件 rango/admin.py。文件中已有一条 import 语句，请修改该模块，以便注册你想要包含的每个类。下面的示例将 Category 和 Page 类都注册到了管理界面。

```python
from django.contrib import admin
from rango.models import Category, Page

admin.site.register(Category)
admin.site.register(Page)
```

未来要添加其他的类，只需再调用一次 admin.site.register() 方法，并确保在模块顶部导入该模型即可。

保存这些更改后，重新启动 Django 开发服务器，并再次访问管理界面 http://127.0.0.1:8000/admin/。你将看到 Category 和 Page 模型，如下所示。

![](img/75169a27287f7c945bf2cbf7dede5251_85_0.png)

Django 管理界面，已包含 Rango 模型。

尝试点击 Rango 部分中的 Categorys 链接。在这里，你应该会看到我们之前通过 Django Shell 创建的 Test 类别。

### 尝试使用管理界面

随着你继续推进 Rango 的开发，你将大量使用管理界面来验证数据是否存储正确。请尝试使用它，了解它的运作方式。这个界面不言自明，容易理解。

删除之前创建的 Test 类别。我们很快会用示例数据填充数据库。你可以从管理界面删除 Test 类别：点击它旁边的复选框，然后从页面顶部的下拉菜单中选择 Delete selected categorys。最后点击出现的红色大按钮来确认你的操作！

### 用户管理

Django 管理界面也是你进行用户管理的首选之地，通过“认证和授权”部分即可实现。在这里，你可以创建、修改和删除用户账户，并调整权限级别。稍后会有更多介绍。

### 复数与单数拼写

注意管理界面中的拼写错误（Categorys，而不是 Categories）。这个拼写错误可以通过在模型定义中添加一个带有 `verbose_name_plural` 属性的嵌套 Meta 类来修复。下面是一个修改后的 Category 模型示例，更多关于 Meta 类中可以存储什么的信息，请参阅 [Django 官方模型文档](https://docs.djangoproject.com/en/2.1/topics/db/models/#meta-options)。

```python
class Category(models.Model):
    name = models.CharField(max_length=128, unique=True)

    class Meta:
        verbose_name_plural = 'Categories'

    def __str__(self):
        return self.name
```

### 扩展 `admin.py`

需要说明的是，Rango 应用中的示例 `admin.py` 模块是目前最简单、最实用的示例。但是，你可以通过多种方式自定义管理界面。如果你感兴趣，可以查看 [官方 Django 管理界面文档](https://docs.djangoproject.com/en/2.1/ref/contrib/admin/) 了解更多信息。在教程的后续部分，我们将探讨如何操作 admin.py 模块。

## 5.7 创建填充脚本

手动向数据库输入测试数据很麻烦。许多开发者会通过随机敲击键盘来添加一些虚假的测试数据，比如 wTFzmN00bz7。与其这样做，不如编写一个脚本来自动用**真实且可信的数据**填充数据库。这是因为当你演示或测试应用时，你需要在数据库中看到一些可信的示例。如果你在团队中工作，自动化脚本意味着每个协作者都可以直接在他们的电脑上运行该脚本，使用与你相同的示例数据初始化数据库。因此，创建一个我们称之为*填充脚本*的东西是一个好习惯。

要创建填充脚本，请新建一个名为 `populate_rango.py` 的文件。将该文件放在 `<workspace>/tango_with_django_project/` 中，也就是你的 Django 项目的根目录下。创建文件后，添加以下代码。

```python
import os
os.environ.setdefault('DJANGO_SETTINGS_MODULE',
    'tango_with_django_project.settings')

import django
django.setup()
from rango.models import Category, Page

def populate():
    # First, we will create lists of dictionaries containing the pages
    # we want to add into each category.
    # Then we will create a dictionary of dictionaries for our categories.
    # This might seem a little bit confusing, but it allows us to iterate
    # through each data structure, and add the data to our models.

    python_pages = [
        {'title': 'Official Python Tutorial',
        'url':'http://docs.python.org/3/tutorial/'},
        {'title':'How to Think like a Computer Scientist',
        'url':'http://www.greenteapress.com/thinkpython/'},
        {'title':'Learn Python in 10 Minutes',
        'url':'http://www.korokithakis.net/tutorials/python/'} ]

    django_pages = [

```

