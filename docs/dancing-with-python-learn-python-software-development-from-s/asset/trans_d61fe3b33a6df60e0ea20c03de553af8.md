

## A.1 操作系统命令行

在本书的若干处，我会要求你在“操作系统命令行”中输入并执行一条语句。例如，在第 10.1.3 节中，我会要求你从命令行运行 `pytest unipoly.py`。

打开命令行的方式取决于你使用的操作系统：

- 对于 Microsoft Windows，请从“开始”菜单打开 **Windows PowerShell** 或 **Windows 命令提示符**，也可以在搜索框中输入“PowerShell”或“命令提示符”。
- 对于 Apple macOS，请从 **Launchpad** 或“应用程序/实用工具”文件夹中启动 **终端**应用程序。
- 对于 Linux，如果你处于图形环境中，可以尝试按 Ctrl-Alt-T 打开**终端**。也可以在系统菜单中寻找“终端”应用程序。如果你不处于图形环境中，那么你已经在终端里了！

在一些操作系统中，命令行也称为“shell 提示符”。

## A.2 安装 Python

你的系统上可能已经安装了 Python 3。尝试在操作系统命令行中运行

```
python -VV
```

或

```
python3 -VV
```

如果你看到 Python 3 应用程序的版本和说明，就表示它已经安装。有些系统可能安装的是 Python 2，此时 `python -VV` 命令可能会启动 Python 2 并显示其信息。尝试运行 `python3 -VV`，查看是否也安装了 Python 3。

本书使用 Python 3.9，有些示例可能无法在更低版本中运行。升级到较新版本的 Python 可能需要特殊权限，才能在计算机上安装软件。若要了解当前的系统升级信息，请在网上搜索以下任一内容：

```
windows upgrade python
mac upgrade python
linux upgrade python
```

然后按照相关说明操作。

若要重新安装 Python，请访问

```
https://www.python.org/downloads/
```

并下载适用于你系统的最新版本。对于 Windows，请务必下载 64 位版本，因为 qiskit 需要该版本。

对于 macOS 和 Windows，请运行下载的软件安装程序。对于 Windows，请选择为所有用户安装并更新 PATH 的选项。

Linux 需要你完成更多安装工作，因此不一定适合新手：

- 下载源代码“tarball”后，使用 tar -xvf，后面跟文件名，解压 Python 文件。
- 进入存放这些文件的文件夹，并在命令行中执行 ./configure。
- 在命令行中运行 make。
- 在命令行中运行 make install。

安装 Python 后，使用 python 或 python3 启动它。具体名称可能因操作系统或安装类型而异。下文中我会使用 python 来指代它。

## A.3 安装 Python 模块和包

PyPI 是“Python 包索引”（Python Package Index），其中包含超过 300,000 个可供下载和导入的模块与包。你可以在以下网址浏览和搜索此目录：

```
https://pypi.org/
```

pip 应用程序可在你的系统上下载和管理 PyPI 包。使用命令行中的 pip 或 pip3 启动该应用程序。下文中我会使用 pip 来展示它的若干功能。运行这些命令时，你看到的输出可能与这里展示的不同，因为我还安装了其他包。如果你感兴趣，可以在附录 F“生产说明”中查看我这台计算机上安装的 Python 包列表。

### 查看 pip 的帮助信息

```
pip --help
```

用法：
  pip <command> [options]

命令：

    install                         安装包。
    download                        下载包。
    uninstall                       卸载包。
    freeze                          以 requirements 格式输出已安装的包。
    list                            列出已安装的包。
    ...

若要查看特定命令的详细帮助信息，请将该命令的名称插入 pip 和 --help 之间。

```
pip list --help
```

```
用法：
    pip list [options]

说明：
    列出已安装的包，包括可编辑安装的包。

包按名称排序，排序时不区分大小写。

列表选项：
    -o, --outdated                 列出过期的包
    -u, --uptodate                 列出最新的包
    -e, --editable                 列出可编辑的项目。
    -l, --local                    如果位于可访问全局包的
                                   虚拟环境中，则不列出
                                   全局安装的包。

...
```

### 列出已安装的 Python 模块和包

```
pip list
```

| 包 | 版本 |
|---|---|
| alabaster | 0.7.12 |
| appdirs | 1.4.4 |
| argon2-cffi | 20.1.0 |
| astroid | 2.5.6 |
| async-generator | 1.10 |
| atomicwrites | 1.4.0 |
| attrs | 21.2.0 |
| ... | ... |

若要查看已过期的包，请添加 --outdated 选项：

| 包 | 版本 | 最新版本 | 类型 |
|---|---|---|---|
| click | 7.1.2 | 8.0.1 | wheel |
| decorator | 4.4.2 | 5.0.9 | wheel |
| h5py | 3.1.0 | 3.2.1 | wheel |
| idna | 2.10 | 3.1 | wheel |
| isort | 4.3.21 | 5.8.0 | wheel |
| ... |  |  |  |

### 安装包

我们安装 **treemap** 模块：

```
pip install treemap
```

Collecting treemap
  Downloading treemap-1.05.tar.gz (5.4 kB)
Building wheels for collected packages: treemap
  Building wheel for treemap (setup.py) ... done
  Created wheel for treemap: filename=treemap-1.5-py3-none-any.whl size=6115

sha256=82cda8fc01b5159ea028900ec198c11a593edd0944223ba636a69850da03cab9
  Stored in directory: c:\users\rssut\appdata\local\pip\cache\wheels\ef\c0\38\d343a9431900820517 18405c15277e4cf5977d0689b7fa345f
Successfully built treemap

Installing collected packages: treemap
Successfully installed treemap-1.5

你可以在 pip install 后列出多个包，一次安装多个包。

### 升级已安装的包

在 pip 后添加 --upgrade 选项，即可升级先前安装的包。对于 **treemap**，我们刚刚才安装，因此无需升级。

```
pip install --upgrade treemap
```

Requirement already satisfied: treemap in d:\python\python39\lib\site-packages (1.5)

**flake8** 模块确实需要升级。从输出中可以看到，pip 会检查 **flake8** 所依赖的包。如果这些依赖包需要升级，pip 也会一并升级。

```
pip install --upgrade flake8
```

```
Collecting flake8
    Using cached flake8-3.9.2-py2.py3-none-any.whl (73 kB)
    Requirement already satisfied: pyflakes<2.4.0,>=2.3.0 in
        d:\python\python39\lib\site-packages (from flake8) (2.3.1)
    Requirement already satisfied: pycodestyle<2.8.0,>=2.7.0 in
        d:\python\python39\lib\site-packages (from flake8) (2.7.0)
    Requirement already satisfied: mccabe<0.7.0,>=0.6.0 in
        d:\python\python39\lib\site-packages (from flake8) (0.6.1)

    Installing collected packages: flake8
      Attempting uninstall: flake8
        Found existing installation: flake8 3.8.4
        Uninstalling flake8-3.8.4:
          Successfully uninstalled flake8-3.8.4
    Successfully installed flake8-3.9.2
```

### 卸载包

我不需要系统上的 **treemap**，因此将其卸载：

```
pip uninstall treemap
```

```
Found existing installation: treemap 1.5
Uninstalling treemap-1.5:
  Would remove:
    d:\python\python39\lib\site-packages\treemap-1.5.dist-info\*
    d:\python\python39\lib\site-packages\treemap\*
    d:\python\python39\scripts\treemap_coverage.exe
    d:\python\python39\scripts\treemap_demo.exe
Proceed (y/n)? y
  Successfully uninstalled treemap-1.5
```

注意，**pip** 会提示我确认是否要删除 **treemap**。我输入“y”，表示“是”。如果我在 pip 命令中加入 --yes 选项，它就不会再要求我确认。

### 更新 pip

虽然 **pip** 随 Python 一同安装，但其开发者更新 pip 的频率比更新 Python 本身更高。你应该定期更新它，以获取最新版本：

```
python -m pip install -U pip
```

Requirement already satisfied: pip in d:\python\python39\lib\site-packages (21.1.2)

在这个例子中，pip 已是最新版本，但我每周都会检查一次。

## A.4 安装虚拟环境

在附录 F“生产说明”中，我列出了安装在我计算机上的 Python 模块，数量将近 200 个。

我真希望能说这些模块彼此配合时都毫无问题，但事实并非如此。开发者更新某个模块时，可能会破坏依赖它的其他代码。

你可以创建较小的虚拟环境，而不是让系统使用一个庞大的 Python 环境。虚拟环境包含 Python 解释器、工具、标准库，以及项目所需的、与之兼容且通过 pip 安装的模块和包。当你同时处理多个需要不同模块集合的项目时，这项功能尤其有用，因为你可以为每个项目分别设置一个虚拟环境。[P.Y.Y]

按照第 A.2 节所述安装 Python 后，你就可以创建虚拟环境。首先，决定要将它放在哪里。常见做法是在主目录下创建一个子目录，并将其命名为 .venv。在 Linux、Mac 或 Windows PowerShell 的命令行中，执行以下命令：

```
cd ~
python -m venv .venv
```

这些命令会将你带到主目录，并安装所需的 Python 核心工具。若要使用此虚拟环境，请输入

```
.venv\Scripts\activate.bat
```

（适用于 Windows），或输入

```
source .venv/bin/activate
```

（适用于 Linux 或 Mac）。

现在，你可以像第 A.3 节中那样使用 pip，安装所需的模块和包。

