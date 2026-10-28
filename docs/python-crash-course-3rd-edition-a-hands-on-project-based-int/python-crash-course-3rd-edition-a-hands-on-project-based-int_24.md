## C 获取帮助

![](img/8f15563c16d2a98c4d3115150679b092_760_0.png)

每个人在学习编程时都会在某一点遇到困难。因此，作为程序员，最重要的技能之一就是如何高效地摆脱困境。本附录概述了几种方法，帮助你在编程遇到困惑时重新上路。

### 第一步

当你遇到困难时，第一步应该是评估自己的情况。在向其他人求助之前，请清晰地回答以下三个问题：

- 你试图做什么？
- 到目前为止你尝试了什么？
- 你得到了什么结果？

请尽量让你的回答具体明确。对于第一个问题，像“我正在尝试在我的新 Windows 笔记本电脑上安装最新版本的 Python”这样的明确陈述，其详细程度足以让 Python 社区的其他人帮助你。像“我正在尝试安装 Python”这样的陈述则无法提供足够的信息让他人提供多少帮助。

你对第二个问题的回答应提供足够的细节，这样你才不会被告知去重复你已经尝试过的事情：“我访问了 https://python.org/downloads，点击了适用于我系统的下载按钮，然后运行了安装程序”比“我访问了 Python 网站，下载了一些东西”更有帮助。

对于第三个问题，知道你收到的确切错误消息会很有帮助，这样你可以用它们在网上搜索解决方案，或者在求助时提供这些信息。

有时，在你向他人求助之前，仅仅回答这三个问题就能让你看到自己遗漏的东西，并帮助你摆脱困境，而无需进行下去。程序员甚至为此取了个名字：橡皮鸭调试法。这个想法是，如果你向一只橡胶鸭（或任何无生命物体）清晰地说明你的情况，并提出一个具体问题，你往往能自己回答自己的问题。一些编程团队甚至会放一只真正的橡胶鸭，以鼓励人们“和鸭子说话”。

### 再试一次

仅仅回到起点再试一次，就足以解决许多问题。假设你正尝试根据本书中的示例编写一个 for 循环。你可能只是遗漏了一些简单的东西，比如 for 行末尾的冒号。再次逐步操作可能会帮助你避免重蹈覆辙。

搜索确切的错误消息也非常有用。例如，假设你在新的 Windows 系统上从终端运行一个 Python 程序时收到以下错误：

```
> python hello_world.py
Python was not found; run without arguments to install from the Microsoft Store...
```

搜索完整短语“Python was not found; run without arguments to install from the Microsoft Store”可能会得到一些好建议。

当你开始搜索编程相关主题时，有几个网站会反复出现。我将简要介绍其中一些网站，以便你了解它们可能有多大帮助。

### Stack Overflow

*Stack Overflow* ([https://stackoverflow.com](https://stackoverflow.com)) 是程序员中最受欢迎的问答网站之一，它经常出现在 Python 相关搜索结果的首页上。成员们在遇到困难时发布问题，其他成员则尽力提供有用的回答。用户可以为他们觉得最有帮助的回答投票，因此最好的答案通常是你最先找到的那些。

许多基础的 Python 问题在 Stack Overflow 上都有非常清晰的答案，因为社区一直在不断完善它们。用户也被鼓励发布更新，因此回答往往能保持相对时新。在撰写本文时，Stack Overflow 上已经回答了近两百万个与 Python 相关的问题。

在 Stack Overflow 上发帖之前，你需要了解一个期望。问题应该是你所面临问题的最短示例。如果你发布了 5–20 行能够生成你遇到的错误的代码




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



--pretty=oneline 标志提供了两个最重要的信息：提交的引用 ID 以及为该提交记录的消息。

## 第二次提交

要了解版本控制的真正威力，我们需要对项目进行更改并提交该更改。这里我们只是在 *hello_git.py* 中添加另一行：

```python
print("Hello Git world!")
print("Hello everyone.")
```

当我们检查项目状态时，我们会看到 Git 注意到了已更改的文件：

```text
git_practice$ git status
① On branch main
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)

②  modified:   hello_git.py

③ no changes added to commit (use "git add" and/or "git commit -a")
git_practice$
```

我们可以看到正在工作的分支 ①、被修改的文件名 ②，以及没有任何更改被提交 ③。让我们提交更改并再次检查状态：

```text
① git_practice$ git commit -am "Extended greeting."
  [main 945fa13] Extended greeting.
  1 file changed, 1 insertion(+), 1 deletion(-)
```

```text
(use "git add <file>..." to update what will be committed)
(use "git restore <file>..." to discard changes in working directory)

①  modified:   hello_git.py

no changes added to commit (use "git add" and/or "git commit -a")
git_practice$
```

Git 看到我们修改了 *hello_git.py* ①，如果我们愿意可以提交该更改。但这次，我们不会提交更改，而是回到上一次我们知道项目可用的提交。我们不会对 *hello_git.py* 做任何事情：我们不会删除该行，也不会使用文本编辑器中的撤销功能。相反，在你的终端会话中输入以下命令：

```text
git_practice$ git restore .
git_practice$ git status
On branch main
nothing to commit, working tree clean
git_practice$
```

命令 `git restore filename` 允许你放弃特定文件中自上次提交以来的所有更改。命令 `git restore .` 放弃所有文件中自上次提交以来的所有更改；此操作将项目恢复到上次提交的状态。

当你返回文本编辑器时，你会看到 *hello_git.py* 已更改回以下内容：

```python
print("Hello Git world!")
print("Hello everyone.")
```

虽然在这样简单的项目中返回到之前的状态似乎微不足道，但如果我们正在处理一个包含数十个修改文件的大型项目，自上次提交以来更改过的所有文件都将被恢复。此功能非常有用：你在实现新功能时可以随意进行尽可能多的更改，如果它们不起作用，你可以丢弃它们而不会影响项目。你不必记住这些更改并手动撤销它们。Git 会为你完成所有这些工作。

> **NOTE**
>
> *你可能需要在编辑器中刷新文件才能看到恢复后的版本。*

## 检出之前的提交

你可以使用 checkout 命令通过使用引用 ID 的前六个字符来重新访问日志中的任何提交。在检出并查看较早的提交后，你可以返回到最新的提交，或者放弃最近的工作并从较早的提交开始开发：

```text
git_practice$ git log --pretty=oneline
945fa13af128a266d0114eebb7a3276f7d58ecd2 (HEAD -> main) Extended greeting.
cea13ddc51b885d05a410201a54faf20e0d2e246 Started project.
git_practice$ git checkout cea13d
Note: switching to 'cea13d'.
① You are in 'detached HEAD' state. You can look around, make experimental changes and commit them, and you can discard any commits you make in this state without impacting any branches by switching back to a branch.

   If you want to create a new branch to retain commits you create, you may do so (now or later) by using `-c` with the switch command. Example:

   git switch -c <new-branch-name>

② Or undo this operation with:

   git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at cea13d Started project.
git_practice$
```

当你检出之前的提交时，你会离开主分支并进入 Git 所称的 *detached HEAD* 状态 ①。HEAD 是项目当前已提交的状态；你处于 *detached* 状态，因为你已离开一个命名分支（此处为 `main`）。

要返回到 `main` 分支，你可以按照建议 ② 撤销之前的操作：

```text
git_practice$ git switch -
Previous HEAD position was cea13d Started project.
Switched to branch 'main'
git_practice$
```

此命令将你带回 `main` 分支。除非你想使用 Git 的一些更高级功能，否则最好不要在检出之前的提交时对项目进行任何更改。但是，如果你是唯一在项目上工作的人，并且你想丢弃所有较新的提交并返回到之前的状态，你可以将项目重置到之前的提交。从 `main` 分支开始，输入以下内容：

```text
① git_practice$ git status
On branch main
nothing to commit, working directory clean
② git_practice$ git log --pretty=oneline
```

再次跟踪你的更改。以下是整个过程在终端会话中的样子：

```text
① git_practice$ git status
On branch main
nothing to commit, working directory clean
② git_practice$ rm -rf .git/
③ git_practice$ git status
fatal: Not a git repository (or any of the parent directories): .git
④ git_practice$ git init
Initialized empty Git repository in git_practice/.git/
⑤ git_practice$ git status
On branch main
No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    .gitignore
    hello_git.py

nothing added to commit but untracked files present (use "git add" to track)
⑥ git_practice$ git add .
git_practice$ git commit -m "Starting over."
[main (root-commit) 14ed9db] Starting over.
  2 files changed, 5 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 hello_git.py
⑦ git_practice$ git status
On branch main
nothing to commit, working tree clean
git_practice$
```

我们首先检查状态，看到我们的工作目录是干净的 ①。然后我们使用命令 `rm -rf .git/` 删除 .git 目录（Windows 上为 `del .git`）②。当我们删除 .git 文件夹后检查状态时，我们被告知这不是一个 Git 仓库 ③。Git 用于跟踪仓库的所有信息都存储在 .git 文件夹中，因此删除它会删除整个仓库。

我们随后可以自由使用 `git init` 开始一个全新的仓库 ④。检查状态显示我们已回到初始阶段，等待第一次提交 ⑤。我们添加文件并进行第一次提交 ⑥。现在检查状态显示我们在新的 `main` 分支上，没有任何内容需要提交 ⑦。

使用版本控制需要一些练习，但一旦开始使用它，你将再也不想没有它工作。
