
# Vim 命令行选项和配置

## 远程服务器选项

以下是一系列相关选项，用于使用服务器 Vim 的远程实例。`--remote` 选项告诉远程 Vim（可能在同一台机器上运行，也可能不在）编辑文件或在该远程服务器中求值表达式。`--server` 选项告诉 Vim 发送到哪个服务器，或指示 Vim 可以将自身声明为服务器。`--serverlist` 仅列出可用的服务器：

* `--remote` `file`
* `--remote-expr` `expr`
* `--remote-send` `keys`
* `--remote-silent` `file`
* `--remote-tab`
* `--remote-tab-silent`
* `--remote-tab-wait`
* `--remote-tab-wait-silent`
* `--remote-wait` `file` …
* `--remote-wait-silent` `file` …
* `--serverlist`
* `--servername` `name`

如需更完整地讨论所有命令行选项，包括完整的 `vi` 集合，请参阅 [“命令行语法”](#app01.html#vi8-app-a-sect-1) 部分。有关 `--remote` 选项的更多信息，请执行 Vim 命令 `:help remote`。

## 与命令名称关联的行为

Vim 主要有两种风格：图形化（在 Unix 变体中使用 X Window 系统，在其他操作系统中使用原生 GUI）和文本化，每种都可以在启动时具有特征子集。Unix 用户只需使用以下列表中的其中一个命令即可获得所需的行为：

`vim`:
启动基于文本的 Vim。

`gvim`:
启动 Vim 的图形模式。在许多环境中，`gvim` 是 Vim 的不同二进制文件，其所有 GUI 选项在编译期间均已打开。这与使用 `vim -g` 启动 Vim 的效果相同。

`view`, `gview`:
以只读模式启动 Vim 或 `gvim`。这与使用 `vim -R` 启动 Vim 的效果相同。

`rvim`:
以受限模式启动 Vim。所有对 shell 命令的外部访问均被禁用，以及使用 `^Z` 命令挂起编辑会话的能力。

`rgvim`:
这与使用 `rvim` 启动 Vim 的效果相同，但适用于图形版本。

`rview`:
类似于 `view`，但以受限模式启动。在受限模式下，用户无法访问过滤器、外部环境或操作系统功能。这与使用 `vim -Z` 启动 Vim 的效果相同（`-R` 选项仅调用前面描述的只读效果）。

`rgview`:
这与使用 `rview` 启动 Vim 的效果相同，但适用于图形版本。

`evim`, `eview`:
使用“简易”模式进行编辑或只读查看。Vim 设置选项和功能，使其对不熟悉 Vim 模式的人来说行为更直观。这与使用 `vim -y` 启动 Vim 的效果相同。专家用户可能不会觉得此模式容易，因为他们已经习惯了标准的 `vi` 行为。

请注意，这些命令没有类似的 `gXXX` 版本，因为据推测 `gvim` 本身已经很简单，或者至少易于学习，具有可预测的点按行为。

`vimdiff`, `gvimdiff`:
以“diff”模式启动并对输入文件执行差异比较。这在 [“有什么不同？”](#ch13.html#vi8-ch-13-sect-8) 部分后面有深入介绍。

`ex`, `gex`:
使用行编辑 `ex` 模式。在脚本中很有用。这与使用 `vim -e` 启动 Vim 的效果相同。

MS-Windows 用户可以在程序列表（开始菜单）中访问类似的 Vim 版本选择。

## 系统和用户配置文件

Vim 按特定顺序查找初始化提示。它执行找到的第一组指令（以环境变量或文件的形式）并开始编辑。因此，以下列表中遇到的第一个元素是唯一会被执行的列表元素。顺序如下：

1. `VIMINIT`: 这是一个环境变量。如果它非空，Vim 会将其内容作为 `ex` 命令执行。
2. User `vimrc` files: `vimrc`（Vim 资源）初始化文件是一个跨平台概念，但由于操作系统和平台的细微差异，Vim 会按以下顺序在不同位置查找它：

`$HOME/.vimrc` Local `*.exrc` and `vimrc` files: 如果设置了 Vim `exrc` 选项，Vim 会查找另外三个配置文件：`*.vimrc`；`*.gvimrc`；和 `*.exrc`。在非 POSIX 系统上，文件名可能以除句点之外的字符开头。

`*.vimrc` 文件是配置 Vim 编辑特性的好地方。几乎任何 Vim 选项都可以在此文件中设置或取消设置，并且特别适合设置全局变量和定义函数、缩写、键映射等。以下是有关 `*.vimrc` 文件的一些注意事项：

* 注释以双引号 (`"`) 开头，双引号可以在行的任何位置。双引号及其之后的所有文本都会被忽略。
* `ex` 命令可以带或不带冒号指定。例如，`set` `autoindent` 与 `:set autoindent` 相同。
* 如果将大量选项定义拆分为单独的行，文件会更易于管理。例如：

```
set terse sw=1 ai ic wm=15 sm nows ruler wc=<Tab> more
```

等同于：

```
set terse     " short error and info messages
set shiftwidth=1
set autoindent
set ignorecase
set wrapmargin=15
set nowrapscan  " don't scan past end or top of file in searches
set ruler
set wildchar=<TAB>
set more
```

注意第二组命令的可读性好得多。第二种方法在通过删除、插入以及在调试配置文件设置时临时注释掉行时也更容易维护。例如，如果你想在启动配置中临时禁用行号，只需在配置文件中的 `set number` 行开头插入双引号 (`"`) 即可。

### 环境变量

许多环境变量会影响 Vim 的启动行为，甚至某些编辑会话行为。这些大多是透明的，如果未配置则使用默认值处理。

#### 如何设置环境变量

登录时拥有的命令环境（在 Unix 中称为 shell）设置变量以反映或控制其行为。环境变量尤其强大，因为它们会影响命令环境中调用的程序。以下说明并非 Vim 特有；可用于设置命令环境中所需的任何环境变量：

MS-Windows:
要设置环境变量：
1. 打开控制面板。
2. 双击 `System`。
3. 点击 `Advanced` 选项卡。
4. 点击 `Environment Variables` 按钮。

结果是一个窗口分为两个环境变量区域，`User` 和 `System`。初学者不应修改 `System` 环境变量。在 `User` 区域，你可以设置与 Vim 相关的环境变量并使其在登录会话之间保持不变。

Unix/Linux Bash 及其他 Bourne shell：
编辑适当的 shell 配置文件（如 Bash 用户的 `*.bashrc`）并插入类似的行：

```
VARABC=somevalue VARXYZ=someothervalue MYVIMRC=/path/to/my/vimrc/file
export VARABC VARXYZ MYVIMRC
```

这些行的顺序无关紧要。`export` 语句只是使变量对在 shell 中运行的程序可见，从而将其转换为环境变量。导出变量的值可以在导出之前或之后设置。

Unix/Linux C shell：
编辑适当的 shell 配置文件（如 `*.cshrc`）并插入类似以下的行：

```
setenv VARABC somevalue
setenv VARXYZ someothervalue
setenv MYVIMRC /path/to/my/vimrc/file
```

#### 与 Vim 相关的环境变量

以下列表显示了大多数 Vim 环境变量及其作用。
