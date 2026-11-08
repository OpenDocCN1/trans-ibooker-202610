
[VsVim](https://github.com/VsVim/VsVim) 是开源扩展，提供在 Visual Studio 内部使用的 Vim 模拟器。截至本文撰写时，它支持 Visual Studio 2017 和 2019，并且项目正在积极开发和维护中。

如果你必须使用 Visual Studio 但更喜欢 Vim 风格的编辑，应该查看一下这个。

# Visual Studio Code 的 Vim

### 注意

我们讨论面向 Visual Studio *和* Microsoft Visual Studio Code 的 Vim 插件。这是因为它们不同。虽然它们经常被混淆为同一应用程序，但事实并非如此，因此插件也不同。

## Visual Studio Code：快速介绍

Visual Studio Code 是 Microsoft 其旗舰 IDE Visual Studio 的轻量级版本。常被称为 *VS Code*。VS Code 与其非自由的商业大哥一样，是一个强大的开发和项目管理集成生态系统。虽然它提供了许多相同的功能，但差异足以使 Vim 插件选项也不同。这大多是吹毛求疵，因为在任一应用程序中添加 Vim 插件都很容易。

显然，第一步是 [下载并安装](https://code.visualstudio.com) VS Code。安装非常简单直接。

## VS Code 扩展

VS Code 的附加组件可互换地称为 *扩展* 或 *插件*。最快访问 VS Code 功能的方法是知道并使用通用的“执行此操作”命令。只需在 Microsoft Windows 和 GNU/Linux 上输入命令 `CTRL SHIFT-P`，或在 MacOS 上输入 `COMMAND SHIFT-P`。（有趣的是，`F1` 在三个操作系统上也能执行此操作。）开始输入 `install extensions`，VS Code 将显示下拉选择列表。参见 [图 16-3](#ch16.html#vi8-ch-16-fig-3)。

![](assets/lvv8_1603.png)

VS Code 显示一个垂直的左侧窗口，汇总已安装的扩展并提供对已安装扩展以及（庞大的）插件生态系统中可用扩展的搜索。在搜索框中输入 `vim`，你应该会看到类似 [图 16-4](#ch16.html#vi8-ch-16-fig-4) 的内容。

![](assets/lvv8_1604.png)

高亮的项目 `vscodevim` 是插件选择。点击“Install”按钮。这会弹出 [图 16-5](#ch16.html#vi8-ch-16-fig-5) 所示的对话框。

![](assets/lvv8_1605.png)

任何时候你想禁用或卸载扩展，都可以像之前一样找到该扩展。这会弹出 [图 16-6](#ch16.html#vi8-ch-16-fig-6) 中的对话框。现在点击“Disable”或“Uninstall。”

![](assets/lvv8_1606.png)

## vscodevim 设置

要查看 `vscodevim` 插件的可用设置，请使用 VS Code 通用命令 `CTRL SHIFT-P`，并搜索 *设置*。可能有很多，但请选择“Preferences: Open `Settings` (UI)。”参见 [图 16-7](#ch16.html#vi8-ch-16-fig-7)。

![](assets/lvv8_1607.png)

现在在 *设置* 对话框中搜索，你会看到与 `vim` 相关的设置几乎有 100 个，如 [图 16-8](#ch16.html#vi8-ch-16-fig-8) 所示。

![](assets/lvv8_1608.png)

我们把偏好设置留给你自行整理。请注意，许多偏好设置只是提供了一个链接来打开 VS Code 的 JSON 设置文件。不幸的是，一些常见的 Vim 自定义需要你熟悉编辑此文件。

例如，如章节 [“简化 Vim 退出”](#ch14.html#vi8-ch-14-sect-1.1) 中所述，我们喜欢通过在 `vi` 命令模式下将 `q` 和 `Q` 映射到 `:q^M` 和 `:q!^M` 分别来简化退出 Vim（退出 Vim 和 *真正* 退出 Vim！）。^([6](#ch16.html#idm46601070728480)) 你无法在 VS Code 扩展偏好设置中执行此操作。以下是创建映射的 JSON 代码块：

```
"vim.normalModeKeyBindingsNonRecursive": [
        {
                "before": ["q"],
                "commands": [":q"]
        },
        {
                "before": ["Q"],
                "commands": [":q!"]
        },
        {
                "before": ["ctrl+v"],
                "commands": ["Ctrl+Shift+G"]
        }
    ],
```

### 注意

由于此操作发生在 VS Code 内部，IntelliSense 对于补全 `vim.XX` 非常有用，其中 *XX* 是正在使用的 `vscodevim` 设置。请注意，在前面的示例中，我们对 `normalModeKeyBindings` 使用版本 `"NonRecursive"`，以防止 `q` 被重新解释为需要再次映射。这是在 Vim 映射中常见且众所周知的技术，通过在上下文中用 `noremap` 前缀映射命令来定义。（参见 [“使用 map 命令”](#ch07.html#vi8-ch-7-sect-3.2) 以更完整地讨论映射 `vi` 命令。）

## Vim 不仅适用于 VS Code

我们选择 VS Code 来讨论 IDE 的 Vim 插件。VS Code 已变得极受欢迎，因为 Microsoft 积极关注其开发和演进。

然而，VS Code 只是众多 IDE 中的一个，几乎所有 IDE 都提供自己的 Vim 编辑仿真或 readily available 的类 Vim 插件。我们已在 NetBeans、Eclipse、PyCharm 和 JetBrains 中使用并验证了 Vim 插件或仿真。还有许多其他。很可能你的 IDE 也有 Vim 插件。

# Unix 实用工具

`vi` 抽象隐藏在许多你可能不了解的 Unix/Linux 实用工具中。以下是一些实用工具的示例，以及 `vi` 命令如何在不编辑时提高你的效率。

## More or Less？

`more` 是原始的基于屏幕的 *分页器* 程序：一种旨在一次呈现一屏数据的程序。它作为 BSD Unix 的一部分开发，与原始 `vi` 大致同期。除了显示文件内容外，`more` 在未给出任何文件时读取标准输入，使其易于在管道末尾使用。

在 `more` 成为标准一段时间后，`less`（其名称是对 `more` 的双关）作为增强型分页器被编写。今天，两者通常都可在现代系统上获得。

我们认为（具有讽刺意味）`more` 实际上更少，而 `less` 更多。`more` 是具有基本分页和交互的传统 Unix 工具，而 `less` 几乎是一个流编辑器。`less` 缺乏真正的编辑功能，但提供了类似 Vim 的强大导航。

我们中的一位在他的个人电脑（GNU/Linux）上总是将 `more` 重命名为 `more_or_less`，并将 `/usr/bin/less` 链接到 `/usr/bin/more`（从而使所有 `more` 皆为 `less`）。

如果你没有管理权限或共享系统而他人可能反对，你可以使用 `alias` 设置相同的内容，该别名可添加到你的个人 `.bashrc` 文件中：

```
alias 'more=less'
```

### 警告

我们的一位技术审阅者警告说，重命名 `more` 并将 `less` 链接到 `more` 可能会导致意外行为和混淆，可能影响期望 `more` 原始行为的脚本和安装程序的执行。我们同意该警告，此建议值得注意。你应谨慎进行这些修改。

为记录在案，自 `less` 命令出现以来，我们在每台 GNU/Linux 安装上都进行了上述修改，从未注意到系统有任何不良影响。尽管如此，因人而异。

另一个选项是设置 `.bashrc` 文件中的 `PAGER` 环境变量，使 `less` 代替 `more` 使用（至少大部分时间）：

```
export PAGER=less
```

这样，“说的越少越好。”以下是 `less` 的类 Vim 功能：

`b`, `d`

: 

分别向上或向下翻一页（`more` 中不可用）。

`gg`, `G`

: 

分别跳转到输入文件或流的开头或末尾（`more` 中不可用）。

`/pattern`, `?pattern`, `n`, `N`

: 
