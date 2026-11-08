
我们已在第一个命令之后使用了子菜单优先级语法（[![](assets/3.png)](#ch09.html#callout_3)、[![](assets/5.png)](#ch09.html#callout_5) 和 [![](assets/7.png)](#ch09.html#callout_7)），以确保每个新项目的正确顺序。请注意，我们从 `.600` 的第一个定义开始。这确保子菜单项放置在我们定义的第一个项目后面，因为我们没有为 *那个* 优先级分配值，因此它默认为 500。

为了更方便地访问，请点击“剪刀”撕下线以获得您个性化的浮动菜单，如 [图 9-13](#ch09.html#vi8-ch-9-fig-13) 所示。

![](assets/lvv8_0913.png)

## 工具栏

工具栏是允许快速访问程序功能的图标长条。例如在 GNU/Linx 上，`gvim` 在窗口顶部显示 [图 9-14](#ch09.html#vi8-ch-9-fig-14) 所示的工具栏。

![](assets/lvv8_0914.png)

[表 9-2](#ch09.html#vi8-ch-9-tab-2) 显示了工具栏图标及其含义。

表 9-2. `gvim` 工具栏图标及其含义

| 图标 || 描述 || 图标 || 描述 |
| --- | --- | --- | --- |
| ![](assets/openFile.png) || 打开文件对话框 || ![](assets/findNext.png) || 查找搜索模式的下一个出现位置 |
| --- | --- | --- | --- |
| ![](assets/saveCurrentFile.png) || 保存当前文件 || ![](assets/findPrevious.png) || 查找搜索模式的上一个出现位置 |
| ![](assets/saveAllFiles.png) || 保存所有文件 || ![](assets/chooseSessionToLoad.png) || 选择已保存的编辑会话进行加载 |
| ![](assets/Print.png) || 打印缓冲区 || ![](assets/saveCurrentSession.png) || 保存当前编辑会话 |
| ![](assets/Undo.png) || 撤销上次更改 || ![](assets/chooseVimScript.png) || 选择要运行的 Vim 脚本 |
| ![](assets/Redo.png) || 重做上次操作 || ![](assets/makeCurrentProject.png) || 使用 `make` 命令构建当前项目 |
| ![](assets/cutToClipboard.png) || 将选择内容剪切到剪贴板 || ![](assets/buildTags.png) || 为当前目录树构建标签 |
| ![](assets/copyToClipboard.png) || 将选择内容复制到剪贴板 || ![](assets/jumpTagUnderCursor.png) || 跳转到光标下的标签 |
| ![](assets/pasteFromClipboard.png) || 将剪贴板粘贴到缓冲区 || ![](assets/Help.png) || 打开帮助 |
| ![](assets/findReplace.png) || 查找并替换 || ![](assets/searchHelp.png) || 搜索帮助 |

如果这些图标不熟悉或不直观，您可以让工具栏同时显示文本和图标。执行此命令：

```
:set toolbar="text,icons"
```

### 注意

与许多高级功能一样，Vim 要求在编译期间打开工具栏功能，以便不想要它们的人可以通过不包含它们来节省内存。工具栏不存在，除非 `+GUI_GTK`、`+GUI_Athena`、`+GUI_Motif` 或 `+GUI_​Photon` 中的其中一个功能编译到您的 `gvim` 版本中。[附录 D，“vi 和 Vim：源代码和构建”](#app04.html#vi8-app-d) 解释了如何重新编译 Vim，在此过程中会创建 `gvim` 可执行文件的链接。

我们修改工具栏的方式与修改菜单非常相似。事实上，我们使用相同的 `:menu` 命令，但带有额外的语法来指定图形。虽然存在一个算法可以帮助 `gvim` 找到与每个命令关联的图标，但我们建议显式指定图标图形。

`gvim` 将工具栏视为一维菜单。并且正如您可以控制新菜单的从右到左的位置一样，您可以通过在 `menu` 命令前加上一个数字来控制新工具栏条目的位置，该数字决定其位置 *优先级*。与菜单不同，没有创建新工具栏的概念。所有新的工具栏定义都出现在单个工具栏上。添加工具栏选择的语法是：

```
:amenu icon=/some/icon/image.bmp ToolBar.NewToolBarSelection Action
```

其中 `/some/icon/image.bmp` 是包含要在工具栏中显示的工具栏按钮或图像（通常是图标）的文件的路径，`NewToolBarSelection` 是工具栏按钮的新条目，而 `Action` 定义按钮的功能。

例如，让我们定义一个新工具栏选择，当点击或选择时，会在 Windows 中打开一个 DOS 窗口。假设 Windows 路径设置正确，我们将定义我们的工具栏选择以从 `gvim` 中启动 DOS 窗口，通过执行以下操作（这是其 `Action`）：

```
:!cmd
```

对于新选择的工具栏按钮或图像，我们使用一个显示 DOS 命令提示符的图标，如 [图 9-15](#ch09.html#vi8-ch-9-fig-15) 所示，在我们的系统上存储在 `$HOME/dos.bmp` 中。

![](assets/lvv8_0915.png)

执行命令：

```
:amenu icon="c:$HOME/dos.bmp" ToolBar.DOSWindow :!cmd<CR>
```

这会创建一个工具栏条目并在工具栏末尾添加我们的图标。工具栏现在应如下所示 [图 9-16](#ch09.html#vi8-ch-9-fig-16)。新图标出现在工具栏的最右侧。

![](assets/lvv8_0916.png)

## 工具提示

`gvim` 允许您为菜单条目和工具栏图标定义工具提示。菜单工具提示在鼠标悬停在该菜单选择上时显示在 `gvim` 命令行区域。工具栏工具提示在鼠标悬停在工具栏图标上时以图形方式弹出。例如，[图 9-17](#ch09.html#vi8-ch-9-fig-17) 显示了当我们将鼠标放在工具栏的“查找上一个”按钮上时弹出的工具提示。

![](assets/lvv8_0917.png)

`:tmenu` 命令为菜单和工具栏项目定义工具提示。语法是：

```
:tmenu TopMenu.NextLevelMenu.MenuItem tool tip text
```

其中 `TopMenu.NextLevellMenu.MenuItem` 定义菜单从顶级一直级联到您希望为其定义工具提示的菜单项。因此，例如，文件菜单下“打开”菜单项的工具提示将使用以下命令定义：

```
:tmenu File.Open Open a file
```

如果您正在定义工具栏项目，请使用 `ToolBar` 作为顶级“菜单”（工具栏没有真正的顶级菜单）。

让我们为上一节中创建的 DOS 工具栏图标定义一个弹出工具提示。输入命令：

```
:tmenu ToolBar.DOSWindow Open up a DOS window
```

现在当您将鼠标悬停在新添加的工具栏图标上时，您可以看到工具提示，如 [图 9-18](#ch09.html#vi8-ch-9-fig-18) 所示。

![](assets/lvv8_0918.png)

# gvim 在 Microsoft Windows 中

`gvim` 在 MS-Windows 用户中越来越受欢迎。经验丰富的 `vi` 和 Vim 用户会发现 Windows 版本非常出色，而且它可能是所有操作系统中最新的版本。

### 注意

自安装的可执行文件应自动且无缝地将 Vim 集成到 Windows 环境中。如果没有，请参阅 Vim 运行时目录中的 `gui-w32.txt` 帮助文件以获取 `regedit` 说明。由于这涉及编辑 Windows 注册表，如果您对此程序有丝毫的不安，请 *不要* 尝试。您可以找到更有经验的人来帮助您。这是一个常见但非微不足道的练习。

长期使用 Windows 的用户熟悉 *剪贴板*，这是一个存储文本和其他信息以便于复制、剪切和粘贴操作的存储区域。Vim 支持与 Windows 剪贴板的交互。只需在可视模式下突出显示文本并单击“复制”或“剪切”菜单项即可将 Vim 文本存储到 Windows 剪贴板中。然后您可以将该文本粘贴到其他 Windows 应用程序中。

# gvim 在 X Window System 中

熟悉 X 环境的用户可以定义和使用许多可调的 X 功能。例如，您可以使用 `.Xdefaults` 文件中通常定义的标准类定义来定义许多资源。

### 警告
