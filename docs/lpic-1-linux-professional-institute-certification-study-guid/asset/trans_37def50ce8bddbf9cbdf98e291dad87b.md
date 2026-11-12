

Vendor		: openSUSE
Installed Size	: 67.7 MiB
Installed		: 是
Status		: 最新
Source package	: emacs-25.3-lp150.2.3.1.src
Summary		: GNU Emacs 基础包
Description	：
GNU Emacs 编辑器的基本软件包。需要 emacs-x11 或 emacs-nox。

```
$
```

zypper 实用程序易于使用，并不断提供有用的消息来指导你的软件包管理过程。例如，在清单 2.18 中，使用了确定特定文件所属软件包的旧方法（what-provides）。作为回应，zypper 实用程序不仅执行该命令，还提供未来应采用的较新方法的信息。

### 清单 2.18：确定文件所属的软件包

```
$ which emacs
/usr/bin/emacs
$
$ zypper what-provides /usr/bin/emacs
Command 'what-provides' is replaced by 'search --provides --match-exact'.
See 'help search' for all available options.
Loading repository data...
Reading installed packages...
```

| 状态 | 名称 | 摘要 | 类型 |
|---|---|---|---|
| i+ | emacs | GNU Emacs 基础包 | 软件包 |

你可以轻松地通过其手册页面获取 zypper 工具的帮助，并使用 zypper help 获取一般帮助或使用 zypper help 命令获取特定帮助。此外，zypper 实用程序允许你缩短一些命令。例如，你可以将 install 缩短为 in，将 remove 缩短为 re，将 search 缩短为 se，如清单 2.19 所示。

### 清单 2.19：使用 zypper search 命令搜索软件包

```
$ zypper se nmap
Loading repository data...
Reading installed packages...
```

| 状态 | 名称 | 摘要 | 类型 |
|---|---|---|---|
| | nmap | 端口扫描器 | 软件包 |
| | nmap4 | Nmap 的图形前端 | 软件包 |
| | zenmap | Nmap 的图形前端 | 软件包 |

使用 zypper 删除软件包也很简单。命令、过程和实用程序的有用消息示例在清单 2.20 中有截取显示。

### 清单 2.20：使用 zypper remove 命令删除软件包

```
$ sudo zypper remove emacs
[sudo] password for root:
[...]
The following application is going to be REMOVED:
"GNU Emacs"

The following 2 packages are going to be REMOVED:
emacs emacs-x11

2 packages to remove.
After the operation, 99.6 MiB will be freed.
Continue? [y/n/...? shows all options] (y): y
(1/2) Removing emacs-25.3-lp150.2.3.1.x86_64 .....................................[done]
(2/2) Removing emacs-x11-25.3-lp150.2.3.1.x86_64 .............................[done]
There are some running programs that might use files deleted by recent upgrade. You may wish to check and restart some of them. Run 'zypper ps -s' to list these programs.
$
```

一旦你了解何时以及如何使用每个实用程序，使用 RPM、YUM 和 ZYpp 管理软件包就非常简单。Debian 软件包管理也是如此。

## 使用 Debian 软件包

你可能已经猜到，Debian 软件包管理系统主要用于基于 Debian 的 Linux 发行版，例如 Ubuntu。使用此系统，你可以安装、修改、升级和删除软件包。我们将在本节中探讨这个流行的软件包管理系统。

### Debian 软件包文件命名约定

Debian 将应用程序文件打包成单个 .deb 软件包文件以供分发，其使用以下文件名格式：

```
PACKAGE-NAME-VERSION-RELEASE_ARCHITECTURE.deb
```

.deb 软件包的这种文件命名约定与 .rpm 文件格式非常相似。但是，在 ARCHITECTURE 中，通常会找到 amd64，表示它针对 AMD64/Intel64 CPU 架构进行了优化。有时会使用 all，表示该软件包在架构上是中立的。清单 2.21 中显示了几个 .deb 软件包文件。

### 清单 2.21：使用 .deb 文件命名约定的软件包

```
$ ls -1 *.deb
docker_1.5-1build1_amd64.deb
emacs_47.0_all.deb
openssh-client_1%3a7.6p1-4ubuntu0.3_amd64.deb
vim_2%3a8.0.1453-1ubuntu1_amd64.deb
zsh_5.4.2-3ubuntu3.1_amd64.deb
$
```

请记住，软件包命名约定是可接受的标准，但（在一定范围内）软件包开发者不必遵循。因此，你可能会遇到变体。

> 如果要在基于 Debian 的发行版（如 Ubuntu）上获取 Debian 软件包文件的副本，请使用 apt-get download 命令。例如，使用超级用户权限，在命令行中键入 sudo apt-get download vim 以将 vim Debian 软件包文件下载到当前工作目录。

### dpkg 命令集

处理 .deb 文件的核心工具是 dpkg 程序，这是一个命令行实用程序，具有用于在 Linux 系统上安装、更新和删除 .deb 软件包文件的选项。dpkg 命令的基本格式如下：

```
dpkg [OPTIONS] ACTION PACKAGE-FILE
```

ACTION 参数定义要对文件执行的操作。表 2.6 列出了你需要使用的更常见的操作。

### 表 2.6 dpkg 命令操作

| 短选项 | 长选项 | 描述 |
|---|---|---|
| -c | --contents | 显示软件包文件的内容 |
| -C | --audit | 查找已损坏的已安装软件包并建议如何修复它们 |
| N/A | --configure | 重新配置已安装的软件包 |
| N/A | --get-selections | 显示当前已安装的软件包 |
| -i | --install | 安装软件包；如果软件包已安装，则将其升级 |
| -I | --info | 显示未安装软件包文件的信息 |
| -l | --list | 列出与指定模式匹配的所有已安装软件包 |
| -L | --listfiles | 列出与软件包关联的已安装文件 |
| -p | --print-avail | 显示已安装软件包的信息 |
| -P | --purge | 删除已安装的软件包，包括配置文件 |
| -r | --remove | 删除已安装的软件包但保留配置文件 |
| -s | --status | 显示指定软件包的状态 |
| -S | --search | 查找拥有指定文件的软件包 |

每个操作都有一组可用于修改其基本行为的选项，例如强制覆盖已安装的软件包或忽略任何依赖错误。要使用 dpkg 程序，系统上必须有 .deb 软件包可用。通常，你可以在应用程序网站上找到可分发的应用程序 .deb 版本。此外，大多数发行版都维护一个用于下载软件包的中心位置。

![](img/ca7617340ddbe2360d36ce3428729fa9_139_0.png)

Debian 发行版还在 www.debian.org/distrib/packages 提供 Debian 软件包的中央清算所。

获得 .deb 软件包后，你可以使用 dpkg -I 命令查看文件中存储的软件包信息，包括版本号和任何依赖项。示例在清单 2.22 中有截取显示。

### 清单 2.22：使用 dpkg -I 命令查看未安装的 .deb 软件包

```
$ dpkg -I zsh_5.4.2-3ubuntu3.1_amd64.deb
new Debian package, version 2.0.
size 689912 bytes: control archive=2544 bytes.
    909 bytes,    20 lines     control
   3332 bytes,    42 lines     md5sums
[...]
Package: zsh
Version: 5.4.2-3ubuntu3.1
Architecture: amd64
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Installed-Size: 2070
Depends: zsh-common (= 5.4.2-3ubuntu3.1), libc6 (>= 2.15),
libcap2 (>= 1:2.10), libtinfo5 (>= 6)
Recommends: libc6 (>= 2.23), libncursesw5 (>= 6), libpcre3
Suggests: zsh-doc
Section: shells
Priority: optional
Homepage: https://www.zsh.org/
Description: shell with lots of features
 Zsh is a UNIX command interpreter (shell) usable as an
[...]
Original-Maintainer: Debian Zsh Maintainers <pkg-zsh-devel@li[...]
$
```

如果要查看软件包文件的内容，请将 -I 选项替换为 --contents 开关。请注意，你可能需要将输出通过管道传送到分页实用程序（参见第 1 章）以便更轻松地查看。

当你确定已获得正确的软件包时，请使用 dpkg 及其 -i 操作来安装它，如清单 2.23 所示。（请注意，如果软件已安装，此过程会将其升级到软件包文件中的版本。）

### 清单 2.23：使用 dpkg -i 命令安装 .deb 软件包

```
$ sudo dpkg -i zsh_5.4.2-3ubuntu3.1_amd64.deb
Selecting previously unselected package zsh.
(Reading database ... 171250 files and directories currently installed.)
```

