

这种方法非常强大，我想你会看到它如何帮助你组织需要完成的工作，并在阅读本书的过程中跟踪你的进度。

## 设置云项目和代码仓库

本书实际上被组织成一个使用单个看板板跨代码仓库的大型物联网项目。每个代码仓库将有一组任务，这些任务与基于章节的里程碑保持一致，并映射回看板。

这种方法的优点是，一旦你的项目和代码仓库设置妥当，你真正需要做的就是在逐章编写、测试和提交代码的过程中更新任务状态。工作流引擎将自动解析你的任务更新，并为你代表任务的卡片在看板上显示。很 neat，不是吗？

如果你有兴趣使用 GitHub 来设置所有这些，这里有一个简单的 3 步流程可供你开始使用：

- 1. 创建一个 GitHub 账户
  - a. 如有需要，创建一个与 GitHub 账户关联的组织
- 2. 在你的账户（或组织）内，创建以下内容：
  - a. 一个名为 ‘Programming the IoT - Exercises’ 的新私有项目
- b. 一个名为 ‘piot-java-components’ 的新私有 Git 代码仓库
- c. 一个名为 ‘piot-python-components’ 的新私有 Git 代码仓库
- 3. 更新 ‘piot-java-components’ 和 ‘piot-python-components’ 的远程代码仓库
  - a. 从命令行执行以下命令：

```
git remote set-url origin {your new URL}
```

```
git commit -m "Initial commit."
```

```
git push
```

- b. 重要：请确保对 ‘piot-java-components’ 和 ‘piot-python-components’ 都执行此操作，并为每个使用适当的 Git 代码仓库 URL！

> NOTE
> 
> 我写了一份指南来帮助你配置所有这些内容，位于本书的主页（https://programmingtheiot.com/programming-the-iot-book.html）。

一旦你完成上述任务，你的基于云的工作流环境现已设置完成并准备使用。请看图 1-12。它显示了我将在全书中使用的任务模板（GitHub 称之为 ‘Issue’）。这是进入每个任务的内容。

它相当简单，你只需要输入五项：Name、Description、Actions、Estimate 和 Validation。你可以更新它，但我建议对所有卡片坚持使用一个模板。如果你创建了一个没有验证步骤的卡片（出于某种原因），只需输入 ‘N/A’。

Title

Write

Preview

# Description

+   - 

# Actions

+   - 

# Estimate (Small = < 2 hrs; Medium = 4 hrs; Large = 8 hrs)

+   - 

# Tests

+   - 

Styling with Markdown is supported

Submit new issue

Figure 1-12. - Task template

这些类别中的大多数都是不言自明的。但是，为什么 Estimate 只有三个工作量级别？在本書中，大多数活动应花费 ~2 小时或更少（Small），大约半天（Medium），或大约一天（Large）。如果你遇到一个你确定需要超过一天的活动，它应该被分解成更小的任务，符合这三个工作量级别之一。

例如，一个名为 *Integrate IoT solution with three cloud services* 的 ‘task’ 显然代表需要完成的工作，但仅凭名称判断，它明显太大且复杂，不可能是一个单一的工作活动。

我的建议是尽可能编写与特定代码模块，甚至单个方法对齐的任务。我相信你以前听过这个：保持简单。

图 1-13 显示了用第一个编码任务填充的模板示例——创建受限设备应用程序（CDA）。

labbenchstudios commented 3 hours ago • edited

# Description

+   - Create new Python module named ConstrainedDeviceApp with class name ConstrainedDeviceApp.

# Actions

+   - Create class within the module named 'ConstrainedDeviceApp'
+   - Add startApp() method, log info message indicating app was started.
+   - Add stopApp() method, log info message indicating app was stopped.
+   - Add main entry function to enable running as an application.
+   - Main entry function creates instance of ConstrainedDeviceApp and calls startApp(), waits 5 seconds, then calls stopApp().

# Estimate (Small = < 2 hrs; Medium = 4 hrs; Large = 8 hrs)

+   - Small

# Tests

+   - App can be run from command line using the Python 3 interpreter and log output for starting and stopping the app.
+   - App exits cleanly and returns to the command line prompt after logging messages.

# Assignees

labbenchstudios

# Labels

feature

# Projects

Programming the IoT - Exercises

To do

# Milestone

Chapter 2 - CDA

# Linked pull requests

Successfully merging a pull request may close this issue.

None yet

Figure 1-13. - Example of a typical development task

图 1-14 显示了将任务添加为看板卡片的结果。该卡片是在将任务与项目对齐后自动生成的。请注意它已被添加到看板的“To do”列中，因为它是新的且目前没有状态。一旦你开始处理任务并更改其状态，它将移动到 ‘In progress’。

# programmingtheiot

Repositories 2

Packages

People 1

Teams

Projects 2

Settings

# Programming the IoT - Exercises

Updated 23 hours ago

Filter cards

+ Add cards

Fullscreen

Menu

# To do

# In progress

# Done

+ Add column

Create the CDA application wrapper - ConstrainedDeviceApp

constraineddevicesapp#1 opened by labbenchstudios

feature

Chapter 2 - CDA

Automated as To do

Manage

Automated as In progress

Manage

Automated as Done

Manage

Figure 1-14. - The new example task automatically added into the Kanban board

## 使用 Git 远程和分支进行源代码控制

使用 Git 的关键好处之一是能够与其他人协作并将你的本地代码仓库与存储在云中的远程代码仓库同步。如果你以前使用过 Git，你已经熟悉远程和分支。我不会在这里深入细节，但它们是作为你的自动化环境的一部分需要掌握的重要概念。

分支本质上是一种方式，使每个开发人员或团队能够在不影响主代码库的情况下分割他们的工作。在 Git 中，这个默认主分支目前称为 ‘master’，通常用于包含已完成、测试、验证并且——通常——已投入生产 的代码。这是 ‘programmingtheiot-java’ 和 ‘programmingtheiot-python’ 的默认分支，虽然你可以保持原样并简单地在此默认分支上工作，但通常不建议这样做，原因如我所述。

分支策略可能因公司和团队而异，虽然我喜欢使用的方法是每个章节使用一个新分支，然后一旦一切工作正常并通过适当测试，章节分支就会合并到 master 中。从那里，为下一章从合并后的 master 创建一个新分支，以此类推。

这种方法允许你轻松跟踪章节之间的更改，甚至可以返回到早期章节的历史记录，如果你想查看例如第 2 章和第 5 章之间发生了什么更改。在 Eclipse 中，你可以右键单击项目（‘programmingtheiot-java’ 或 ‘programmingtheiot-python’），并选择 ‘Team > Switch To > New Branch...’ 来为你的代码建立一个新分支。

我建议你使用 ‘chapternn’ 作为每个分支名称的命名约定，其中 ‘nn’ 是两位数的章节号。例如，第 1 章的分支将命名为 ‘chapter01’，第 2 章将命名为 ‘chapter02’，以此类推。

> NOTE
> 
> 本书的范围不包括 Git 分支和合并的所有细节，因此如果您想深入了解，我建议阅读以下指南 https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging。

## 云中的自动化 CI/CD

在 Eclipse 中，你可以编写你的 CDA 和 GDA 代码，执行单元测试，并构建和打包这两个应用程序。这实际上并不是自动化的，因为你必须通过从命令行执行类似 mvn install 的命令，或从 IDE 内调用 Maven install 过程来自行启动该过程。这对于将两个应用程序带到可以运行它们的程度非常好，但实际上并没有运行它们 -

