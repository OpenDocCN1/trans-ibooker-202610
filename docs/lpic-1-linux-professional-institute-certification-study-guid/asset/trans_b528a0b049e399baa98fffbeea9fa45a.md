

yum 程序非常通用，表 2.4 显示了您可以与其使用的一些命令。

### 表 2.4 yum 命令

| 命令 | 说明 |
|---|---|
| check-update | 检查存储库中已安装软件包的更新 |
| clean | 删除安装期间下载的临时文件 |
| deplist | 显示指定软件包的依赖项 |
| groupinstall | 安装指定的软件包组 |
| info | 显示有关指定软件包的信息 |
| install | 安装指定的软件包 |
| list | 显示已安装软件包的信息 |
| localinstall | 从指定的 RPM 文件安装软件包 |
| localupdate | 从指定的 RPM 文件更新系统 |
| provides | 显示文件属于哪个软件包 |
| reinstall | 重新安装指定的软件包 |
| remove | 从系统中删除软件包 |
| resolvedep | 显示与指定依赖项匹配的软件包 |
| search | 在存储库软件包名称和描述中搜索指定关键字 |
| shell | 进入 yum 命令行模式 |
| update | 将指定软件包更新到存储库中的最新版本 |
| upgrade | 更新指定软件包但删除过时软件包 |

使用 yum 安装新应用程序轻而易举，如清单 2.13 所示。

### 清单 2.13：在 CentOS 发行版中使用 yum 安装软件

```
# yum install emacs
[...]
Resolving Dependencies
--> Running transaction check
---> Package emacs.x86_64 1:24.3-22.el7 will be installed
--> Processing Dependency: emacs-common = 1:24.3-22.el7 for
package: 1:emacs-24.3-22.el7.x86_64
[...]
package:
--> Running transaction check
---> Package ImageMagick.x86_64 0:6.7.8.9-16.el7_6 will be installed
[...]
--> Finished Dependency Resolution

Dependencies Resolved
==============================================================
Package Arch Version Repository Size
==============================================================
Installing:
emacs x86_64 1:24.3-22.el7 base 2.9 M
Installing for dependencies:
ImageMagick x86_64 6.7.8.9-16.el7_6 updates 2.1 M
[...]
Transaction Summary
==============================================================
Install 1 Package (+8 Dependent packages)

Total download size: 26 M
Installed size: 92 M
Is this ok [y/d/N]: y
Downloading packages:
(1/9): OpenEXR-libs-1.7.1-7.el7.x86_64.rpm | 217 kB 00:01
[...]
(9/9): emacs-common-24.3-22.el7.x86_64.rpm | 20 MB 00:22
--------------------------------------------------------------------------------
Total 1.2 MB/s | 26 MB 00:22
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
[...]
Installing : ImageMagick-6.7.8.9-16.el7_6.x86_64 8/9
Installing : 1:emacs-24.3-22.el7.x86_64 9/9
[...]
Verifying : ImageMagick-6.7.8.9-16.el7_6.x86_64 8/9
Verifying : 1:emacs-24.3-22.el7.x86_64 9/9

Installed:
emacs.x86_64 1:24.3-22.el7

Dependency Installed:
ImageMagick.x86_64 0:6.7.8.9-16.el7_6 [...]
emacs-common.x86_64 1:24.3-22.el7 [...]
libXaw.x86_64 0:1.0.13-4.el7 [...]
libotf.x86_64 0:0.9.13-4.el7 [...]

Complete!
#
```

yum 的一个出色功能是能够将软件包分组以进行分发。
无需下载特定环境所需的所有软件包
（例如，使用 Apache、MySQL 和 PHP 服务器的 Web 服务器），您可以下载
将软件包捆绑在一起的软件包组。使用 yum grouplist 命令查看可用的各种软件包组列表，
并使用 yum groupinstall group-package-name 在系统上更轻松地安装软件包。

> 最近，另一种 RPM 包管理工具正在变得流行。dnf 程序（dandified yum 的缩写）作为 Fedora Linux 发行版的一部分包含在内，用于替代 yum。正如其名称所示，dnf 提供了一些 yum 缺少的高级功能。此类功能之一是加速使用库文件解析依赖项搜索。

yum 的另一个出色功能是能够重新安装软件包。如果发现软件包文件丢失或以某种方式被修改，可以通过软件包重新安装轻松修复。示例代码片段在清单 2.14 中显示。

### 清单 2.14：使用 yum 实用程序重新安装软件

```
# rpm -V emacs
missing    /usr/bin/emacs-24.3
#
# yum reinstall emacs
[...]
---> Package emacs.x86_64 1:24.3-22.el7 will be reinstalled
[...]
Total download size: 2.9 M
Installed size: 14 M
Is this ok [y/d/N]: y

[...]
Installed:
  emacs.x86_64 1:24.3-22.el7

Complete!
#
# rpm -V emacs
#
```

注意在清单 2.14 中，`rpm -V emacs` 命令发现软件包中缺少文件。使用 `yum reinstall` 功能可以快速解决问题。
使用 `yum` 删除软件包与安装它一样容易。示例代码片段在清单 2.15 中显示。

### 清单 2.15：使用 yum 实用程序删除软件

```
# yum remove emacs
[...]
Remove  1 Package

Installed size: 14 M
Is this ok [y/N]: y
[...]
Removed:
  emacs.x86_64 1:24.3-22.el7

Complete!
#
```

通常不需要修改存储在 /etc/yum.conf 文件中的主要 YUM 配置。该文件包含设置（也称为指令），用于确定诸如在哪里记录 YUM 日志数据等事项。虽然您可以通过编辑主要配置文件或手动创建 /etc/yum.repos.d/ 存储库文件来添加第三方存储库，但不建议这样做。推荐的方法是通过 RPM 或 YUM 安装新存储库。

## 使用 ZYpp

openSUSE Linux 发行版使用 RPM 包管理系统并在 .rpm 文件中分发软件，但不使用 yum 或 dnf 工具。相反，openSUSE 创建了自己的包管理工具，称为 ZYpp（也称为 libzypp）。它的 zypper 命令允许您直接从 openSUSE 存储库查询、安装和删除系统上的软件包。表 2.5 列出了更常用的 zypper 实用程序命令。

### 表 2.5 zypper 命令

| 命令 | 说明 |
| :--- | :--- |
| help | 显示总体常规帮助信息或指定命令的帮助 |
| install | 安装指定的软件包 |
| info | 显示有关指定软件包的信息 |
| list-updates | 显示存储库中已安装软件包的所有可用软件包更新 |
| lr | 显示存储库信息 |
| packages | 列出所有可用软件包或列出来自指定存储库的可用软件包 |
| what-provides | 显示文件属于哪个软件包 |
| refresh | 刷新存储库的信息 |
| remove | 从系统中删除软件包 |
| search | 搜索指定的软件包 |
| update | 更新指定的软件包，或者如果未指定软件包，则将当前安装的所有软件包更新到存储库中的最新版本 |
| verify | 验证已安装的软件包是否已满足所需依赖项 |

对于软件包安装，zypper 的操作方式与 yum 实用程序类似。示例代码片段在清单 2.16 中显示。

### 清单 2.16：使用 zypper 实用程序安装软件

```
$ sudo zypper install emacs
[sudo] password for root:
[...]
Reading installed packages...
Resolving package dependencies...

The following 9 NEW packages are going to be installed:
emacs emacs-info emacs-x11 etags liblm17n0 libotf0 libXaw3d8 m17n-db
m17n-db-lang

The following recommended package was automatically selected:
m17n-db-lang

9 new packages to install.
Overall download size: 0 B. Already cached: 27.4 MiB. After the operation,
additional 111.6 MiB will be used.
Continue? [y/n/...? shows all options] (y): y

[...]
Checking for file conflicts: .............................................[done]
[...]
(9/9) Installing: emacs-x11-25.3-lp150.2.3.1.x86_64 ......................[done]
$
```

info 命令很有帮助，因为它显示指定软件包的信息，如清单 2.17 中所示。

### 清单 2.17：使用 zypper info 命令显示软件包信息

```
$ zypper info emacs
[...]
Information for package emacs:
--------------------------------
Repository   : openSUSE-Leap-15.0-Update
Name         : emacs
Version      : 25.3-lp150.2.3.1
Arch         : x86_64
```

