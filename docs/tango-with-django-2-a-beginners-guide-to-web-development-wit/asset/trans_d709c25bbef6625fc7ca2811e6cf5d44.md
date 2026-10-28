

## （不）提交你的数据库

你还应该采取措施忽略存储库中其他会频繁变化的基于二进制的文件。以本书为例，你需要考虑忽略数据库（正如我们在上面演示的那样）。为什么？在团队中工作时，每个人对数据库的副本可能都会有所不同。不同的团队成员可能会添加与其他成员不同的示例数据。如果所有团队成员都不停地推送不同版本的数据库，产生冲突的可能性就会增加。这也是我们鼓励开发一个填充脚本来快速用示例数据填充新数据库的原因！这里的经验法则是：不要将数据库提交到项目存储库中。要提交从头创建数据库所需的基础设施，并且作为额外福利，提供用示例数据填充这个空白数据库的功能。

## 基本命令与工作流程

当你已将存储库克隆到本地计算机并准备就绪后，就可以开始掌握 Git 工作流程了。本节将向你展示基本的 Git 工作流程，以及你可以发出的相关 Git 命令。

![](img/75169a27287f7c945bf2cbf7dede5251_393_0.png)

Git 工作流程图。

我们在上面提供了基本 Git 工作流程的示意图。将黑色圆圈中的每个编号与下方的编号描述对应起来，以了解更多关于每个阶段的信息。**当你对下一步应该做什么不确定时，请参考此图——它非常有用！**

### 1. 起步

在开始项目工作之前，你必须先准备好 Git。如果你还没有整理好项目的 Git 工作区，则需要**克隆你的存储库来进行设置**。

如果你已经克隆了存储库，养成使用 `git pull` 命令更新本地副本的习惯是一个好做法。这会*拉取*远程存储库中的最新更改到你的计算机上。通过这样做，你将使用存储库的最新版本。这将减少文件版本冲突的可能性，而冲突真的会让你的生活变得有些糟糕。

要执行 `git pull`，首先在命令提示符或终端中导航到你的存储库目录，然后发出 `git pull` 命令。查看下面来自 Bash 终端的片段，了解你需要做什么，以及应该看到什么输出。

```
$ cd somerepository/
$ git pull
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/someuser/somerepository
    86a0b3b..a7cec3d  master     -> origin/master
Updating 86a0b3b..a7cec3d
Fast-forward
    README.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

此示例显示，通过最新一次拉取，README.md 文件已被更新或创建。

### 遇到错误？

如果你收到 `fatal: Not a git repository (or any of the parent directories): .git`，说明你不在正确的目录中。你需要 `cd` 到你的本地存储库目录——即你克隆存储库的那个目录。大多数 Git 命令只有在 Git 存储库中才能正常工作。

### 先拉取再推送！

在开始工作之前，始终在你的存储库本地副本上执行 `git pull`。务必如此！在你即将推送之前，再执行一次拉取，以确保你拥有最新的更改。

记住要与团队沟通以协调你的活动，这样你们就不会处理相同的文件，或者使用分支⁹⁹来保持内容独立。分支在某个时候会涉及合并回单个分支。

### 2. 开始工作！

一旦你的工作区已克隆或更新为最新更改，就是时候开始干活了！在你的工作区目录中，你可以修改现有文件。你也可以删除它们，或添加新文件以进行版本控制。

当你以任何方式修改存储库时，都需要让 Git 了解你所做的任何更改——这不会自动发生。告诉 Git 发生了什么变化，可以让你保持本地索引更新。本地索引中存储的文件列表随后将用于执行你的下一次*提交（commit）*，我们将在下一步中讨论。为了让 Git 保持同步，有几个 Git 命令可以让你更新本地索引。其中三个命令与 Unix 速成课程中讨论的命令（例如 cp、mv）几乎相同，只是多了个 git 前缀。

- 第一个命令 `git add` 允许你请求 Git 将某个特定文件添加到*暂存区（staging）*，以供下一次提交使用。对于任何你想要纳入版本控制的文件——无论它是新文件还是已更新的文件，你都必须 `git add` 它。通过输入 `git add <filename>` 来调用该命令，其中 `<filename>` 是你希望添加到下一次提交中的文件的名称。可以使用 `git add .` 命令一次添加多个文件和目录——**但请务必小心**¹⁰⁰。
- `git mv` 的功能与 Unix 的 `mv` 命令相同——它用于移动文件。两者唯一的区别在于，`git mv` 会在移动文件之前为你更新本地索引。使用语法 `git mv <current_filename> <new_filename>` 指定文件名。例如，使用此命令，你可以将文件移动到存储库中的另一个目录。这将在你的下一次提交中体现出来。该命令也用于重命名文件——从当前文件名改为新文件名。
- `git cp` 允许你复制文件或目录，同时为你把新文件的引用添加到本地索引中。语法与上面的 `git mv` 相同，指定文件名或目录名的方式如下：`git cp <current_filename> <copied_filename>`。
- `git rm` 命令将文件或目录的删除请求添加到本地索引中。虽然 `git rm` 命令不会立即删除文件，但在下一次提交时，所请求的文件或目录会从你的文件系统和 Git 存储库中移除。语法与 `git add` 命令类似，可以这样指定文件名：`git rm <filename>`。请注意，你可以一次性向本地索引添加大量请求，而不是手动逐个删除文件。例如，`git rm -rf media/` 会在本地索引中为 `media/` 目录创建删除请求。`r` 开关使 Git 能够*递归地*删除 `media/` 目录中的每个文件，而 `f` 则允许 Git *强制*删除这些文件。有关 `rm` 命令的更多信息，请查看 [Wikipedia 页面](http://en.wikipedia.org/wiki/Rm_(Unix))。

提交之间的大量更改可能会让事情变得相当混乱。例如，你可能会轻易忘记已经指示 Git 删除哪些文件。幸运的是，你可以运行 `git status` 命令查看已从当前工作目录修改但尚未添加到本地索引进行处理的文件列表。看看下面命令的典型输出，以便了解你可以看到什么。

![](img/75169a27287f7c945bf2cbf7dede5251_396_0.png)

### 使用 `.gitignore` 文件

如果你已经[正确设置了 `.gitignore` 文件](#)，你会注意到，当你 `git add` 时，与 `.gitignore` 文件中指定模式匹配的文件会被忽略。这是预期行为——这些文件不应该被提交到版本控制中！然而，如果你确实需要包含一个位于 `.gitignore` 中的文件，你可以在必要时使用 `git add -f <filename>` 命令*强制* Git 包含它。

```
On branch master
Your branch is up-to-date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   chapter-git.md
        modified:   chapter-system-setup.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        images/screenshot_template.ai

no changes added to commit (use "git add" and/or "git commit -a")
```

文件 `chapter-git.md`、`chapter-system-setup.md` 和 `images/screenshot_template.ai` 都列在上面的示例输出中。所有路径都是相对于你当前工作目录的，因此最后一个文件添加了 `images/` 前缀——它位于一个 images 子目录中。

仔细阅读 `git status` 命令的输出，我们可以看到 `chapter-git.md` 和 `chapter-system-setup.md` 文件已被你的 Git 存储库知晓，但它们与上一次提交相比已被*修改*。命令的输出告诉你，为了让这些更改反映在下一次提交中，你必须 `git add` 这些文件。与上一次提交相关的已删除或移动的文件也会出现在这里。

还有一个*未跟踪文件（untracked files）*部分。这些文件从未被提交到版本控制中——因此不会出现在任何 Git 提交中。你刚刚创建的文件会出现在这个部分。要提交它们，你只需依次 `git add` 每个文件。如果你再次运行 `git status`，你会看到它们移到了第一个部分。

### 检查状态

有关 `git status` 命令的更多信息，请查看 [官方 Git 文档](https://git-scm.com/docs/git-status)。

