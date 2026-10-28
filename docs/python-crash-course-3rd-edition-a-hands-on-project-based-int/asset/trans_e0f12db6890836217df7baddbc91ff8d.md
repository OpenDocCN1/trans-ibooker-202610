
在下一章中，你将使用 Django 构建一个 Web 应用程序作为你的最终项目。

[OceanofPDF.com](OceanofPDF.com)

## 18

## 开始使用 Django

![](img/8f15563c16d2a98c4d3115150679b092_601_0.png)

随着互联网的发展，网站和移动应用之间的界限已经模糊。网站和应用都以各种方式帮助用户与数据交互。幸运的是，你可以使用 Django 构建一个既能提供动态网站，又能提供一系列移动应用的单一项目。*Django* 是 Python 最流行的*Web 框架*，是一套用于构建交互式 Web 应用程序的工具。在本章中，你将学习如何使用 Django 构建一个名为“学习日志”（Learning Log）的项目，这是一个在线日志系统，让你能够

记录你所学到的关于不同主题的信息。

我们将为这个项目编写一份规范，然后为应用将要处理的数据定义模型。我们将使用 Django 的管理系统输入一些初始数据，然后编写视图和模板，以便 Django 能构建网站的页面。

Django 可以响应页面请求，并简化数据库的读写、用户管理以及更多操作。在第 19 章和第 20 章中，你将完善“学习日志”项目，然后将其部署到正式服务器上，这样你（以及世界上的其他人）都可以使用它。

## 设置项目

在开始像 Web 应用这样重要的项目时，你首先需要在一份规范（specification）或*规格说明*中描述项目的目标。一旦你有了清晰的目标，就可以开始确定实现这些目标的可管理任务。

在本节中，我们将为“学习日志”编写一份规范，并开始项目的第一阶段。这将涉及设置虚拟环境以及构建 Django 项目的初始内容。

### *编写规范*

一份完整的规范会详细说明项目目标、描述项目功能，并讨论其外观和用户界面。与任何好的项目或商业计划一样，规范应该让你保持专注，并帮助你的项目不偏离轨道。我们不会在这里编写完整的项目规范，但我们会列出

几个明确的目标，以保持开发过程的聚焦。以下是我们将使用的规范：

我们将编写一个名为“学习日志”的 Web 应用，允许用户记录他们感兴趣的主题，并在学习每个主题时写日志条目。“学习日志”的主页将描述该网站，并邀请用户注册或登录。登录后，用户可以创建新主题、添加新条目，以及阅读和编辑现有条目。

当你研究一个新主题时，维护一份关于所学内容的日志可以帮助你跟踪新信息和已经找到的信息。在学习技术主题时尤其如此。一个好的应用，比如我们即将创建的这个，可以帮助提高这一过程的效率。

## 创建虚拟环境

要使用 Django，我们首先需要设置一个虚拟环境。*虚拟环境*是你系统中的一个位置，你可以在其中安装包，并将它们与所有其他 Python 包隔离。将一个项目的库与其他项目分开是有益的，而且在我们于第 20 章将“学习日志”部署到服务器时，这也是必要的。

为你的项目创建一个名为 *learning_log* 的新目录，在终端中切换到该目录，然后输入以下代码来创建虚拟环境：

```
learning_log$ python -m venv ll_env
learning_log$
```

这里我们正在运行 `venv` 虚拟环境模块，并使用它创建一个名为 *ll_env* 的环境（请注意，这个名称以两个小写的 L 开头，而不是两个数字 1）。如果你在运行程序或安装包时使用类似 `python3` 的命令，请确保在这里使用该命令。

## 激活虚拟环境

现在我们需要使用以下命令激活虚拟环境：

```
learning_log$ source ll_env/bin/activate
(ll_env)learning_log$
```

该命令会运行 ll_env/bin/ 中的 activate 脚本。当环境处于激活状态时，你会看到环境名称出现在括号中。这表示你可以向该环境安装新包，并使用已经安装的包。当环境未激活时，你在 ll_env 中安装的包将不可用。

> 注意
> 如果你使用的是 Windows，请使用命令 ll_env\Scripts\activate（不包括单词 source）来激活虚拟环境。如果你使用的是 PowerShell，你可能需要将 Activate 大写。

要停止使用虚拟环境，请输入 deactivate：

```
(ll_env)learning_log$ deactivate
learning_log$
```

当你关闭运行虚拟环境的终端时，该环境也会变为非激活状态。

## 安装 Django

在虚拟环境激活的状态下，输入以下内容来更新 pip 并安装 Django：

```
(ll_env)learning_log$ pip install --upgrade pip
(ll_env)learning_log$ pip install django
Collecting django
--snip--
Installing collected packages: sqlparse, asgiref, django
Successfully installed asgiref-3.5.2 django-4.1 sqlparse-0.4.2
(ll_env)learning_log$
```

由于 pip 会从各种来源下载资源，因此它升级得相当频繁。每当你创建新的虚拟环境时，最好升级一下 pip。

我们现在在虚拟环境中工作，所以安装 Django 的命令在所有系统上都是一样的。不需要使用更长的命令，比如 python -m pip install package_name，也不需要包含 --user 标志。请记住，只有在 ll_env 环境处于激活状态时，Django 才可用。

注意

> Django 大约每八个月发布一个新版本，因此你在安装 Django 时可能会看到更新的版本。这个项目很可能像这里写的那样运行，即使是在更新版本的 Django 上也是如此。如果你想确保使用与这里看到的相同版本的 Django，请使用命令 pip install django==4.1.*。这将安装 Django 4.1 的最新版本。如果你在使用版本时遇到任何问题，请参阅本书的在线资源：https://ehmatthes.github.io/pcc_3e。

## 在 Django 中创建项目

在保持虚拟环境激活的状态下（记得在终端提示符中寻找括号中的 ll_env），

输入以下命令来创建一个新项目：

```
1  (ll_env)learning_log$ django-admin startproject ll_project .
2  (ll_env)learning_log$ ls
ll_env ll_project manage.py
3  (ll_env)learning_log$ ls ll_project
__init__.py asgi.py settings.py urls.py wsgi.py
```

startproject 命令 ① 告诉 Django 创建一个名为 ll_project 的新项目。命令末尾的句点 (.) 会创建新项目，并采用便于开发完成后将应用部署到服务器的目录结构。

注意
不要忘记这个句点，否则你在部署应用时可能会遇到一些配置问题。如果你忘记了句点，请删除已创建的文件和文件夹（除了 ll_env），然后重新运行该命令。

运行 ls 命令（Windows 上是 dir）② 会显示 Django 已创建了一个名为 ll_project 的新目录。它还创建了一个 manage.py 文件，这是一个短程序，用于接收命令并将其传送给 Django 的相关部分。我们将使用这些命令来管理任务，例如操作数据库和运行服务器。

ll_project 目录包含四个文件 ③；其中最重要的是 settings.py、urls.py 和 wsgi.py。settings.py 文件控制 Django 如何与你的系统交互并管理你的项目。随着项目的发展，我们将修改一些设置，并添加一些我们自己的设置。urls.py 文件告诉 Django 响应浏览器请求时应构建哪些页面。wsgi.py 文件帮助

Django 提供它创建的文件。该文件名是“web server gateway interface”（Web 服务器网关接口）的缩写。

## 创建数据库

Django 将项目的大部分信息存储在数据库中，所以接下来我们需要创建一个 Django 可以使用的数据库。输入以下命令（仍然在激活的环境中）：

```
(ll_env)learning_log$ python manage.py migrate
① Operations to perform:
    Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
    Applying contenttypes.0001_initial... OK
    Applying auth.0001_initial... OK
    --snip--
    Applying sessions.0001_initial... OK
② (ll_env)learning_log$ ls
db.sqlite3 ll_env ll_project manage.py
```

每当我们修改数据库时，我们就是在*迁移*数据库。首次发出 `migrate` 命令会让 Django 确保数据库与项目的当前状态匹配。当我们首次在一个使用 SQLite 的新项目中运行此命令时（稍后会详细介绍 SQLite），Django 会为我们创建一个新的数据库。在这里，Django 报告它将准备数据库以存储处理管理和身份验证任务所需的信息 ①。

运行 `ls` 命令会显示 Django 创建了另一个名为 *db.sqlite3* 的文件 ②。*SQLite* 是一种基于单个文件的数据库；它非常适合编写简单的应用，因为你不需要花太多精力来管理数据库。

