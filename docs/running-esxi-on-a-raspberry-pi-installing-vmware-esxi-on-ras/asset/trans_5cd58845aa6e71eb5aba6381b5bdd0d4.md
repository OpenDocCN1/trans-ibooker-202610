

desktops，厚客户端可能运行一些独立的应用程序，例如办公应用程序，并且可能提供本地存储。

## 原生客户端

访问虚拟桌面不需要特殊设备，许多公司允许——甚至可能鼓励——最终用户从标准笔记本电脑或台式机访问其虚拟桌面。VDI 厂商通过在 Windows、Linux、macOS 或其他操作系统上运行的应用程序来实现此功能（见图 11-10）。

![](img/430161835ae267467687add4a1cdc429_272_0.png)

图 11-10. 原生客户端

原生客户端可能（取决于安全要求）允许集成功能，例如从本地应用程序向虚拟桌面剪切和粘贴。

## Web 客户端

随着 HTML5 的出现，网络浏览器很快成为访问虚拟桌面的有吸引力的方式，现在所有主要 VDI 厂商都支持基于 Web 的客户端。虽然它们可能不具备前面列出的其他客户端的所有功能，但它们确实提供了非常好的用户体验，并且在无需物理设备或安装原生客户端的情况下提供了访问虚拟桌面的灵活性。许多自带设备（BYOD）的用户更喜欢通过 Web 客户端访问，因为它不需要他们在设备上安装任何额外软件。Web 客户端与虚拟桌面之间的连接如图 11-11 所示。

![](img/430161835ae267467687add4a1cdc429_273_0.png)

图 11-11. Web 客户端

## 一体式客户端

出于便利目的，许多制造商已将 VDI 客户端与显示器无缝结合。这不仅腾出了桌面空间，而且还使设备更容易维护，因为它可以作为一个整体进行更换。

图 11-12 显示了一台专门为医疗保健行业设计的 LG 一体式设备。它内置射频识别（RFID）读卡器，用于基于接近度的免提登录，以及其他使其对该市场具有吸引力的功能。

![](img/430161835ae267467687add4a1cdc429_274_0.png)

*图 11-12. LG 一体式设备*

## 移动设备

平板电脑、手机和其他移动设备也可以用于访问虚拟桌面。事实上，一些 VDI 厂商甚至允许将“捏合”等原生手势传递到移动设备。一些厂商支持通过 Web 客户端访问虚拟桌面，而其他厂商则专门为移动设备设计了原生客户端。

能够通过移动设备访问虚拟桌面对用户来说是一种解放的体验，因为他们无需携带专用设备。作为一个主要例子，如果医生在参加活动时需要访问患者的记录，他们可以立即从手机上完成，而无需返回办公室。

## 远程协议的作用

在继续之前，我应该解释一下远程协议到底做什么。以最简单的形式，如图 11-13 所示，远程协议将一个系统的显示传输到另一个系统显示；它还将本地系统的键盘和鼠标发送到远程系统。这使用户能够以与本地系统完全相同的方式与远程系统进行交互。

![](img/430161835ae267467687add4a1cdc429_275_0.png)

图 11-13. 远程协议

远程协议还可能执行其他功能，例如为安全目的加密数据流、压缩数据流以降低网络要求、将本地机器的 USB 设备传递到虚拟桌面、从虚拟桌面向本地连接的打印机打印，以及许多其他服务。

## 连接到虚拟桌面

在本节中，我将讨论一些连接到虚拟桌面的方法。我将首先使用 RDP，因为它是最常用和最广泛可用的协议。随后我将使用 X11 连接到桌面，X11 是 Unix 和 Linux 系统最早使用的协议之一。最后，我将使用 NoMachine（NX），这是一种专有但流行的协议，用于连接到 Windows 和 Linux 桌面。我将使用 Windows 10 系统连接到 Linux VM。

## Linux VM

Windows 无疑是最流行的桌面操作系统；然而，微软尚未发布可在通用 Arm 处理器系统上运行的 Windows 公共版本。然而，Linux 社区已完全拥抱 Arm 处理器，所有主要 Linux 发行版都有带桌面功能的版本。还有许多 Linux 发行版专门致力于使用 Linux 提供类似 Windows 的桌面体验。在本节中，我们将查看为 Linux VM 安装附加软件包以使其适合桌面使用的操作。

### Fedora

要使用 Fedora VM，您可以通过以下步骤在其上安装图形桌面：

1. 从控制台登录到您的 Fedora 实例。
2. 切换到 root 用户，然后输入 sudo su。
3. 通过输入以下命令安装 GNOME 桌面和 GUI 应用程序

```
dnf -y group install "Basic Desktop" GNOME
dnf -y install libreoffice
dnf -y install xeyes
```

4. 通过输入以下命令禁用 pcscd 服务

```
systemctl stop pcscd; systemctl disable pcscd
```

5. 通过输入 startx 启动图形桌面。
6. 您将在控制台中看到一个基本的图形桌面。单击 Activities。
7. 启动图形终端。在桌面顶部的搜索框中，输入 XTerm。

![](img/430161835ae267467687add4a1cdc429_277_0.png)

8. 在 XTerm 终端中，通过输入 /usr/bin/xeyes 启动图形应用程序。
9. 验证眼睛是否跟随鼠标。
10. 关闭 XTerm。

### Ubuntu

要使用 Ubuntu VM，它需要安装图形桌面。Ubuntu 有不同的桌面可以安装，以下是安装基本 GNOME 桌面的说明：

1. 从控制台登录到您的 Ubuntu 实例。
2. 切换到 root 用户，然后输入 sudo su。
3. 通过输入以下命令安装 GNOME 桌面和一些应用程序

```
apt install -y gnome-session gdm3
apt install -y libreoffice
apt install -y x11-apps
apt-get install -y gnome-terminal
apt-get install -y links2
apt-get install -y firefox
```

4. 通过输入以下命令设置图形桌面默认启动

```
systemctl set-default graphical.target
```

5. 您将在控制台中看到一个基本的图形桌面。通过输入 init 6 重启系统。
6. 系统启动后，登录到 GNOME 桌面。

![](img/430161835ae267467687add4a1cdc429_278_0.png)

7. 单击 Activities。

### NoMachine for ARM - arm64

![](img/430161835ae267467687add4a1cdc429_280_0.png)

16. 通过输入以下命令安装 NoMachine

```
sudo su
apt install -y nomachine
sudo dpkg -i nomachine_7.6.2_3_arm64.deb
```

## 使用 RDP 连接到 Fedora 桌面

RDP 是最初与 Windows 机器一起使用的协议。它最初被称为 Terminal Services，并基于 Citrix 的 MultiWin 技术。

所有 Windows 系统都附带 RDP 客户端，可以通过在 Windows 命令行中输入 mstsc.exe 启动。RDP 客户端和服务器已为包括 Fedora 在内的各种机器开发。

### 在 Fedora 上安装 RDP 服务器

要在 Fedora 系统上安装 RDP 服务器，请执行以下步骤：

1. 使用 SSH 或控制台登录到 Fedora 系统。
2. 通过输入以下命令安装 RDP 服务器、启用它并为其打开防火墙

```
dnf -y install xrdp tigervnc-server
systemctl enable --now xrdp
firewall-cmd --add-port=3389/tcp --permanent
firewall-cmd --reload
```

### 使用 RDP 连接到 Linux 桌面

您可以按照以下步骤从 Windows 机器连接到 Fedora 机器：

1. 从 Windows 开始菜单，输入 mstsc.exe。
2. 在远程桌面连接面板中，输入 Fedora 系统的 IP 地址。

![](img/430161835ae267467687add4a1cdc429_281_0.png)

3. 输入您的用户名和密码并单击 **OK**。

![](img/430161835ae267467687add4a1cdc429_282_0.png)

4. 短暂等待后，您应该会收到“Welcome to GNOME”消息。单击 **Take Tour**。

