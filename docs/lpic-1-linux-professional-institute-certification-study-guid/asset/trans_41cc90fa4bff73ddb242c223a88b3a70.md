

- 库依赖项：软件包数据库跟踪每个应用程序所需的库文件，并在您安装软件包时依赖的库文件不存在时发出警告。

- 应用程序版本：软件包数据库跟踪应用程序的版本号，以便您知道何时有应用程序的更新版本可用。

以下各节将讨论使用这些软件包管理系统的工具。

## 使用 RPM

在 Red Hat 开发的 RPM 软件包管理器 (RPM) 实用程序可让您安装、修改和删除软件包。它还简化了更新软件的过程。

![](img/ca7617340ddbe2360d36ce3428729fa9_120_0.png)

递归缩写将缩写本身作为组成它的单词的一部分。Linux 世界中的一个著名示例是 GNU，它代表“GNU 不是 Unix”。RPM 是一个递归缩写。

## RPM 发行版和约定

Red Hat Linux 发行版以及其他基于 Red Hat 的发行版（如 Fedora 和 CentOS）使用 RPM。此外，还有其他非基于 Red Hat 的发行版，例如 openSUSE 和 OpenMandriva Lx，也使用 RPM。

RPM 软件包文件具有 .rpm 文件扩展名，并遵循以下命名格式：

```
PACKAGE-NAME-VERSION-RELEASE.ARCHITECTURE.rpm
```

**PACKAGE-NAME** PACKAGE-NAME 与您所期望的一样——即软件包的名称。例如，如果您想要安装 emacs 文本编辑器，则其 RPM 文件的软件包名称很可能是 emacs。但是，请注意，不同的发行版可能为同一程序使用不同的 PACKAGE-NAME，且软件包名称可能与程序名称不同。

**VERSION** VERSION 是程序的版本号，表示比旧版本号更新的软件修改。传统上，软件包的版本号格式为由点（.）分隔的两到三个数字和/或字母。示例包括 1.13.1 和 7.4p1。

**RELEASE** RELEASE 也称为构建号。它表示比版本号更小的程序修改。此外，由于持续软件交付模式的兴起，您通常会在点号后的发行版本号中找到版本控制系统（VCS）编号。示例包括 22 和 94.gitb2f74b2。某些发行版在构建号中包含发行版版本。例如，您会在点号后找到 el7（Red Hat Enterprise Linux v7）或 fc29（Fedora，以前称为 Fedora Core，v29）。

**ARCHITECTURE** 这是软件包所针对的 CPU 架构的标识。通常对于 64 位处理器，您会看到列出的 x86_64。有时会使用 noarch，表示软件包在架构上是中立的。旧的 CPU 架构标识包括 i386（x86）、ppc（PowerPC）以及 i586 和 i686（Pentium）。

> RPM 软件包有两种类型：源代码包和二进制包。大多数情况下，您需要二进制包，因为它包含成功运行软件所需的程序包。源代码 RPM 包含程序的源代码，可用于分析（或用于整合您自己的软件包自定义内容）。您可以通过以下方式区分这两种软件包文件类型：源代码 RPM 的 ARCHITECTURE 在 RPM 文件名中为 src。

查看一些示例 RPM 文件很有帮助。清单 2.1 显示了我们在 CentOS 发行版上下载的四个不同的 RPM 文件。

### 清单 2.1: 在 CentOS 发行版上查看 RPM 软件包文件

```
# ls -l *.rpm
docker-1.13.1-94.gitb2f74b2.el7.centos.x86_64.rpm
emacs-24.3-22.el7.x86_64.rpm
openssh-7.4p1-16.el7.x86_64.rpm
zsh-5.0.2-31.el7.x86_64.rpm
#
```

请注意版本号和发行版本号之间的命名格式差异。虽然确定版本号何时结束、发行版本号何时开始可能比较困难，但诀窍是在文件名中查找第二个短划线（-），它将它们分隔开。

> 如果要在基于 Red Hat 的发行版（如 CentOS 或 Fedora）上获取 RPM 文件副本，请使用 yumdownloader 实用程序。例如，使用超级用户权限并在命令行中输入 **yumdownloader emacs**，将 emacs RPM 文件下载到当前工作目录。在 openSUSE 上，您需要使用超级用户权限执行 zypper install -d package-name 命令。这将把 RPM 软件包文件下载到 /var/cache/zypp/packages/ 子目录中。

## rpm 命令集

处理 RPM 文件的主要工具是 *rpm* 程序。*rpm* 实用程序是一个命令行程序，用于安装、修改和删除 RPM 软件包。其基本格式如下：

```
rpm ACTION [OPTION] PACKAGE-FILE
```

*rpm* 命令的一些常见操作在表 2.1 中有所描述。

### 表 2.1 rpm 命令操作

| 短选项 | 长选项 | 描述 |
| --- | --- | --- |
| -e | --erase | 删除指定的软件包 |
| -F | --freshen | 仅在存在早期版本时才升级软件包 |
| -i | --install | 安装指定的软件包 |
| -q | --query | 查询指定的软件包是否已安装 |
| -U | --upgrade | 安装或升级指定的软件包 |
| -V | --verify | 验证软件包文件是否存在以及软件包的完整性 |

## 安装和更新 RPM 软件包

要使用 *rpm* 命令，您必须将 .rpm 软件包文件下载到系统中。虽然可以使用 `-i` 操作安装软件包，但更常见的是使用 `-U` 操作，它会安装新软件包，或在软件包已安装时进行升级。

![](img/ca7617340ddbe2360d36ce3428729fa9_122_0.png)

> 安装或更新软件包时，您始终需要获取超级用户权限。许多其他软件包管理命令也需要这些权限。您通常可以通过登录 root 账户或使用 sudo 实用程序获得所需权限，后者要求您的账户已配置为能够执行此操作（有关详细信息，请参见第 10 章）。

添加 -vh 选项是一种常用的组合，可显示更新的进度及其正在执行的操作。清单 2.2 展示了示例。请注意，您需要使用超级用户权限来安装和/或更新软件包。

### 清单 2.2: 安装/升级 RPM 软件包文件

```
# rpm -Uvh zsh-5.0.2-31.el7.x86_64.rpm
Preparing...                ########################################### [100%]
Updating / installing...
   1:zsh-5.0.2-31.el7        ########################################### [100%]
#
```

没人愿意输入那些冗长的软件包文件名。由于有大量的短划线、点号和数字，很容易出现拼写错误。相反，请使用 shell 的命令补全功能（也称为 Tab 自动补全）。输入软件包文件名的 PACKAGE-NAME 部分，然后按 Tab 键。只要没有名称相似的其他文件，shell 就会为您完成软件包文件名的其余部分。这是一个很棒的功能！

## 查询 RPM 软件包

使用 -q 操作可针对已安装软件包的软件包管理数据库执行简单查询。清单 2.3 显示了示例。请注意，对于已安装的软件包（例如 zsh），会显示完整的软件包文件名（不带 .rpm 文件扩展名）。

### 清单 2.3: 对 RPM 软件包执行简单查询

```
# rpm -q zsh
zsh-5.0.2-31.el7.x86_64
#
# rpm -q docker
package docker is not installed
#
```

您可以向查询操作添加多个选项以获取更详细的信息。表 2.2 列出了一些更常用的查询选项。

### 表 2.2 rpm 命令查询操作选项

| 短选项 | 长选项 | 描述 |
| --- | --- | --- |
| -c | --configfiles | 列出软件包配置文件的名称和绝对目录引用 |
| -i | --info | 提供详细信息，包括版本、安装日期和签名 |
| N/A | --provides | 显示软件包提供的功能 |
| -R | --requires | 显示各种软件包要求（依赖项） |
| -s | --state | 提供软件包中不同文件的状态，例如正常（已安装）、未安装或已替换 |
| N/A | --what-provides | 显示文件属于哪个软件包 |

-qi 选项提供大量有关软件包的信息，如清单 2.4 所示。

### 清单 2.4: 对 RPM 软件包执行详细查询

```
# rpm -qi zsh
Name        : zsh
Version     : 5.0.2
Release     : 31.el7
Architecture: x86_64
Install Date: Tue 09 Apr 2019 02:51:26 PM EDT
Group       : System Environment/Shells
Size        : 5854390
License     : MIT
Signature   : RSA/SHA256, Mon 12 Nov 2018 09:49:55 AM EST, Key ID 24c6a[...]
Source RPM  : zsh-5.0.2-31.el7.src.rpm
Build Date  : Tue 30 Oct 2018 12:48:17 PM EDT
Build Host  : x86-01.bsys.centos.org
Relocations : (not relocatable)
Packager    : CentOS BuildSystem <http://bugs.centos.org>
Vendor      : CentOS
URL         : http://zsh.sourceforge.net/
Summary     : Powerful interactive shell
Description :
The zsh shell is a command interpreter usable as an interactive login
[...]
#
```

请注意，通过此详细查询，您可以确定软件包的版本号、安装日期、签名等信息。但是，缺少一些数据项，例如软件包的依赖项。

