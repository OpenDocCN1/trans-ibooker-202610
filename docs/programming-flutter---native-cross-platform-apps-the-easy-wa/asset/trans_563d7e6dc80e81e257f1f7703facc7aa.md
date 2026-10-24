

## 官方 Zip 文件的替代方案

在 Gentoo 和 Arch Linux 上，你可以使用包来安装 Android SDK 和/或 Android Studio：

- 在 Gentoo 上，你可以通过运行 `emerge --ask dev-util/android-studio` 来安装 Android Studio 包 [4]；
- 对于 Arch Linux，AUR 上有一个维护活跃且流行的包叫做 *android-studio* [5]。

首先前往 Android Developers 页面上的下载页面。[6] 在那里你会找到 Android Studio 的 .zip 下载链接（大小约为 1GB）。

无论你选择哪种方式，在 64 位操作系统上让 SDK 正常工作，你可能需要安装一些 32 位库：

- 在 Debian/Ubuntu 上，运行 `sudo apt install libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1 libbz2-1.0:i386`
- 在 Fedora 上，运行 `sudo dnf install zlib.i686 ncurses-libs.i686 bzip2-libs.i686`。

之后，我们就可以实际安装 Android SDK 了。

要使用完整的 Android Studio 安装程序进行安装，在从 Google 网站下载的 zip 文件解压后，运行 `bin` 子目录中的 `studio.sh` 脚本。这将启动一个设置向导。完成 SDK 的设置后，你可以通过运行同一个 `studio.sh` 脚本来启动 Android Studio。

## 在 Linux 上安装 Flutter SDK

本部分将为初学者提供面向 Linux 的安装指导；如果你熟悉命令行，CLI 步骤会更可预测。对于完全的 Linux 新手来说，遵循 GUI 导向的指导可能更容易，因为那通常更熟悉。

### Tarball 的替代方案

安装官方 tarball 有替代方案：

- Arch Linux 在 AUR 中有一个维护活跃的包用于安装 Flutter。[7]

Linux 下载是一个源 tarball，其中还包含运行 flutter 命令所需的脚本，这意味着你只需解压它并将解压后 tarball 的 bin 子目录添加到 PATH 环境变量中即可。

在我们这样做之前，需要浏览到 Flutter 官方网站上的 SDK 归档[8] 页面并下载最新稳定版。

或者，在仅 CLI 环境中，你可以使用 curl https://storage.googleapis.com/flutter_infra/releases/stable/linux/flutter_linux_v1.2.1-stable.tar.xz -o flutter_linux_v1.2.1-stable.tar.xz 下载 1.2.1 tarball。

使用你喜欢的任何 GUI 工具解压你刚刚下载的 tarball，或运行以下命令：

```
$ tar -xf flutter_linux_v1.2.1-stable.tar.xz
```

现在我们需要将可执行脚本添加到 PATH 环境变量中。

在此之前，你需要记下你解压 tarball 的目录。它包含一个 flutter 目录，其中有一个 bin 目录；我们需要知道到达该 bin 目录的路径。

如果你使用 GUI，在大多数发行版中它可以通过浏览目录的属性获得。如果按照我们之前描述的步骤中每个软件的默认设置进行操作，它将类似于 /home/username/Downloads/flutter_linux_v1.9.1-stable/flutter/bin。我建议将其移动到更永久的路径；最好是你会记得你安装了 Flutter 的位置。

如果你使用 CLI 并按照上面概述的命令操作，浏览到你的 Flutter 安装目录，然后通过运行以下命令将工作目录更改为 flutter/bin 子目录：

```
$ cd flutter/bin
```

并通过运行以下命令获取工作目录：

```
$ pwd
```

这将返回类似 /home/username/flutter/bin 的内容（这里你会看到你安装 Flutter 的路径，所以你的情况可能差异很大）。

## 了解你的 Shell

### 了解你的 Shell

本节假设你的 shell 是 Bash。[9]
对于大多数 Linux 发行版（以及通常的类 Unix 操作系统——一些例外是 BSD，它们使用 tcsh,[10] ksh,[11] 或 ash,[12] 以及 Arch Linux 的安装程序，它运行在 Zsh[13] 上但默认安装 Bash）来说都是如此，而且由于你需要手动安装和配置不同的 shell，所以你可能知道如何向其 PATH 中添加目录。

要将此添加到 PATH 环境变量，我们需要编辑 ~/.bash_profile。

要使用 GUI 执行此操作，你首先需要让文件管理器显示隐藏文件。如果你找不到主目录中名为 .bash_profile 的文件，你需要切换使文件管理器显示隐藏文件的选项，这取决于你使用的文件管理器：

- 在 Nautilus（又名 GNOME Files，默认用于使用 GNOME 桌面的大多数发行版，如 Ubuntu、RHEL、Fedora，以及默认的 SLED 和 Debian）中，你需要按 Ctrl + H。此快捷键在 PCManFM（LXDE 的一部分，如 Lubuntu 中）、Caja（Mate 的一部分）和 Thunar（XFCE 桌面的部分，如 Xubuntu 中）中也适用。
- 如果你使用的是 KDE 桌面（例如运行 Kubuntu 或在安装 openSUSE 或 Debian 等发行版时选择它）及其默认的 Dolphin 文件管理器，请使用 Alt + 。

一旦你找到名为 .bash_profile 的文件，使用任何文本编辑器打开它，并在末尾新增一行添加以下内容：

```
export PATH=$PATH:/home/username/etc
```

其中你将 /home/username/etc 替换为你之前记下的字符串。

如果你更喜欢使用命令行或只是想从本安装指南中复制粘贴体验，你可以改为打开一个终端窗口或 TTY 并运行以下命令：

```
$ echo "export PATH=$PATH:/home/username/etc/> " >>~/.bash_profile
```

将 /home/username/etc/ 替换为你之前为解压的 tarball 的 flutter/bin 目录找到的路径。

为了能够实际运行 flutter 命令，你需要通过运行以下命令刷新终端的配置：

```
$ source ~/.bash_profile
```

## 在 Windows 上安装

就像在 Linux 上一样，Windows 仅限的 Android 开发也是受支持的，因此我们将安装仅 Android SDK、Android Studio 和 Flutter SDK 本身。

### 在 Windows 上安装 Android SDK

要在 Windows 上安装 Android SDK，你需要做的就是下载并运行官方下载页面上提供的 Android Studio 安装文件。[14] 这也会安装功能丰富的 Android Studio IDE。安装 Android Studio 后，你将被引导完成 Android SDK 的安装。

### 在 Windows 上安装 Flutter SDK

要在 Windows 上安装 Flutter SDK，你首先需要先安装 Git for Windows。你可以在其官方下载页面上找到它的安装文件。[15] 在安装过程中，你需要选择 Use Git from the Windows Command Prompt 选项。

安装 Git 后，从其官方安装页面下载 Flutter 的最新版本。[16] 这与 Windows 上大多数软件不同，将下载一个 zip 归档（又名压缩文件夹），其中包含一个 flutter 文件夹：将它解压到你想要的任何位置（这将是任何 IDE 插件会要求你输入的 SDK 文件夹）。

如果你想在 Flutter 控制台上运行命令，运行 `bin` 子文件夹中包含的 `flutter.bat` 脚本。

## 在 macOS 上安装

macOS 的优势在于它也支持 iOS 构建和调试。为了利用这一点，我们还需要安装 Xcode 和 iOS SDK。

### 安装 Xcode 和 iOS SDK

要能够运行 Flutter 并将其用于 iOS 开发，你需要从 Apple 官方网站下载 Xcode [19] 和 iOS SDK [18]。

### 在 macOS 上安装 Android SDK

在 macOS 上安装 Android SDK 非常简单：前往 Android Studio 下载页面 [19] 下载 .dmg 文件，该文件还包含功能丰富但资源密集的 Android Studio IDE。

### 在 macOS 上安装 Flutter SDK

在 macOS 上安装 Flutter 与在 Linux 上安装 Flutter 非常相似。主要区别在于 macOS 使用 zip 归档格式而不是更高效的 tar.xz 格式，并且没有发行版之间需要处理的 OS 或 GUI 差异。

首先从 Flutter 官方网站下载最新的稳定版 .zip。[20]

将其解压到任何目录，然后打开你刚刚解压的 `flutter` 目录。按 ⌘ + ↖ 并记下 *Where:* 右侧出现的路径。

现在，打开一个终端窗口并运行命令：

```bash
$ nano ~/.bash_profile
```

执行之后，将以下内容粘贴到终端窗口中：

```bash
export PATH=$PATH/example/path/to/flutter/bin
```

