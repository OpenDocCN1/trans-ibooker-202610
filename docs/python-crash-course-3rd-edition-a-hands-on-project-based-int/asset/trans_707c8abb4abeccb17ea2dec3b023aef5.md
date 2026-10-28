

它的许多高级工具可帮助团队在大型项目上协作，但其最基本的功能同样适用于独立开发者。Git 通过跟踪项目中每个文件的更改来实现版本控制；如果犯了错误，你只需恢复到之前保存的状态即可。

## 安装 Git

Git 可以在所有操作系统上运行，但在每个系统上的安装方式有所不同。以下各节提供了针对每个操作系统的具体说明。

某些系统默认自带 Git，而且 Git 常常与你可能已安装的其他软件包捆绑在一起。在尝试安装 Git 之前，先检查你的系统上是否已经安装了它。打开一个新的终端窗口，执行命令 `git --version`。如果看到输出了具体的版本号，说明 Git 已安装在你的系统上。如果看到提示你安装或更新 Git 的消息，请按照屏幕上的说明操作。

如果你没有看到任何屏幕提示，并且你使用的是 Windows 或 macOS，可以从 https://git-scm.com 下载安装程序。如果你是使用 apt 兼容系统的 Linux 用户，可以通过命令 `sudo apt install git` 安装 Git。

## 配置 Git

Git 会跟踪谁对项目做出了更改，即使项目只有一个人在做。为此，Git 需要知道你的用户名和电子邮件地址。你必须提供用户名，但可以编造一个假的电子邮件地址：

```
$ git config --global user.name "username"
$ git config --global user.email "username@example.com"
```

如果忘了这一步，Git 会在你首次提交时提示你提供这些信息。

最好还为每个项目的主分支设置默认名称。这个分支的一个好名字是 main：

```
$ git config --global init.defaultBranch main
```

此配置意味着你使用 Git 管理的每个新项目都将从一个名为 main 的单一提交分支开始。

## 创建一个项目

让我们来创建一个项目用于练习。在系统上某个位置创建一个名为 `git_practice` 的文件夹。在该文件夹中，创建一个简单的 Python 程序：

`hello_git.py`

```python
print("Hello Git world!")
```

我们将用这个程序来探索 Git 的基本功能。

## 忽略文件

扩展名为 `.pyc` 的文件是由 `.py` 文件自动生成的，因此我们不需要 Git 跟踪它们。这些文件存储在一个名为 `__pycache__` 的目录中。要让 Git 忽略这个目录，请创建一个名为 `.gitignore` 的特殊文件——文件名以点号开头且没有扩展名——并在其中添加以下一行：

`.gitignore`

```
__pycache__/
```

这个文件告诉 Git 忽略 `__pycache__` 目录中的任何文件。使用 `.gitignore` 文件可以让你的项目保持整洁，也更容易处理。

你可能需要修改文件浏览器的设置，以便显示隐藏文件（文件名以点号开头的文件）。在 Windows 资源管理器中，勾选“查看”菜单中标签为**隐藏项目**的复选框。在 macOS 上，按 ⌘-SHIFT-（点号）。在 Linux 上，查找标签为“显示隐藏文件”的设置。

> **注意**
> 
> *如果你使用的是 macOS，请向 `.gitignore` 中再添加一行。添加名称 `.DS_Store`；这些隐藏文件包含 macOS 上每个目录的信息，如果你不将它们添加到 `.gitignore` 中，它们会使项目变得杂乱。*

## 初始化仓库

现在你有了一个包含 Python 文件和 `.gitignore` 文件的目录，可以初始化一个 Git 仓库了。打开终端，导航到 *git_practice* 文件夹，然后运行以下命令：

```
git_practice$ git init
Initialized empty Git repository in git_practice/.git/
git_practice$
```

输出显示 Git 已在 *git_practice* 中初始化了一个空仓库。*仓库*是 Git 正在积极跟踪的程序中的一组文件。Git 用来管理仓库的所有文件都位于该隐藏目录中。

我们执行命令 `git commit -m "message"` ① 来对项目做一个快照。`-m` 标志告诉 Git 将后面的消息（Started project.）记录到项目日志中。输出显示我们位于 `main` 分支 ② 上，并且有两个文件已更改 ③。

现在检查状态时，我们可以看到位于 `main` 分支上，并且工作树是干净的 ④。每当你提交项目的一个可用状态时，都应该看到这条消息。如果看到不同的消息，请仔细阅读；这很可能是因为你在提交前忘记添加某个文件了。

## 查看日志

Git 保存了项目所有提交的日志。让我们查看日志：

```
git_practice$ git log
commit cea13ddc51b885d05a410201a54faf20e0d2e246 (HEAD -> main)
Author: eric <eric@example.com>
Date:   Mon Jun 6 19:37:26 2022 -0800

    Started project.
git_practice$
```

每次提交时，Git 都会生成一个唯一的 40 字符引用 ID。它记录谁进行了提交、提交时间以及记录的消息。你并不总是需要所有这些信息，因此 Git 提供了一个选项来打印更简略的日志条目版本：

```
git_practice$ git log --pretty=oneline
cea13ddc51b885d05a410201a54faf20e0d2e246 (HEAD -> main) Started project.
git_practice$
```

