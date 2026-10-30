
Fedora 34（服务器版）
内核 5.11.12-300.fc34.aarch64，运行于 aarch64（tty1）

Web 控制台：https://fedora:9090/ 或 https://10.0.0.234:9090/

Fedora 登录：

- 3. 我以所创建的用户身份登录了 Web 控制台。

![](img/430161835ae267467687add4a1cdc429_172_0.png)

- 4. 登录 Web 控制台后，我可以在**真实主机名**文本框中输入主机名来设置它。

![](img/430161835ae267467687add4a1cdc429_172_1.png)

5. 在日志中，我注意到 SELinux 已启用。

![](img/430161835ae267467687add4a1cdc429_173_0.png)

6. SELinux 是一个强大的安全工具，但我发现它有时限制过度。由于我的环境相当安全，我选择移除它。在“应用程序”选项卡中，我选中它并点击**移除**。

![](img/430161835ae267467687add4a1cdc429_173_1.png)

7. 在右上角，我从下拉菜单中选择“关机”。在主机客户端中，我使用“编辑设置”将虚拟机改为 1 个 CPU 和 2 GB 内存。这是必要的，因为我计划在 ESXi 服务器上运行多台虚拟机。

Fusion）或受支持的公共云之一。它可以免费获取，易于在 vSphere 上安装，并可用作项目的基础操作系统。它的安装和使用方式与其他 Linux 发行版略有不同。

有适用于 Raspberry Pi 的镜像和完整的 Arm64 ISO 镜像。Pi 镜像用于在 Pi 上原生运行；你需要安装完整的 Arm64 Photon OS 4.0 GA 版本。

Arm 镜像的下载地址是 https://github.com/vmware/photon/wiki/Downloading-Photon-OS

![](img/430161835ae267467687add4a1cdc429_175_0.png)

以下是在你的 Pi 上安装 Photon OS 的步骤。

## 下载 Photon OS

我们将直接把安装 ISO 下载到 ESXi 数据存储中。之后我们将使用该 ISO 安装系统。

- 1. SSH 登录 ESXi 系统。
- 2. 输入以下命令获取 root 用户权限：

  `sudo bash`

## 第 8 章　ESXi 上的虚拟机

- 3. 输入以下命令切换到要存储 ISO 的目录：

  `cd /vmfs/volumes/rPi-USB-Datastore-01`

- 4. 输入以下命令打开防火墙以允许 HTTP 连接：

  `esxcli network firewall ruleset set --enabled=true --ruleset-id=httpClient`

- 5. 输入以下命令下载 Arm 镜像：

  `wget https://packages.vmware.com/photon/4.0/GA/iso/photon-4.0-1526e30ba-aarch64.iso`

![](img/430161835ae267467687add4a1cdc429_176_0.png)

该 ISO 大小为 2.9 GB，下载需要几分钟。

你也可以将 ISO 下载到桌面系统，然后使用 SCP 将其复制到 ESXi 主机。

## 创建 Photon 虚拟机

- 1. 登录 ESXi 主机客户端。
- 2. 在导航器中，选择**虚拟机**，然后点击**创建/注册虚拟机**。
- 3. 在“新建虚拟机”向导中，选择“创建新虚拟机”，然后点击“下一步”。

![](img/430161835ae267467687add4a1cdc429_177_0.png)

- 4. 在“名称”文本框中输入 PhotonOS40。在“客户机操作系统系列”下拉菜单中选择 Linux。在“客户机操作系统版本”下拉菜单中选择 VMware Photon OS（64 位），然后点击“下一步”。

![](img/430161835ae267467687add4a1cdc429_177_1.png)

- 5. 确认 rPi-USB-Datastore-01 是数据存储，然后点击**下一步**。
- 6. 将虚拟硬件设置为 3 个 CPU、4 GB 内存和 32 GB 硬盘。
- 7. 展开**CD/DVD 驱动器 1**，选择**数据存储 ISO 文件**，然后选择你的 Photon OS ISO 镜像。确认已选中**开机时连接**。
- 8. 展开**硬盘 1**，选择**精简置备**。点击**下一步**。

## 自定义设置

配置虚拟机硬件和虚拟机的其他选项。

![](img/430161835ae267467687add4a1cdc429_178_0.png)

- 9. 确认设置并点击**完成**。

## 在虚拟机上安装 Photon

我将使用之前安装的 VMRC 来安装操作系统。

- 1. 右键单击 Photon OS 虚拟机，然后选择**启动远程控制台**。

![](img/430161835ae267467687add4a1cdc429_179_0.png)

- 2. 在 VMRC 顶部，点击**开机**图标。
- 3. 接受 EULA（最终用户许可协议）。

## 第 8 章　ESXi 上的虚拟机

![](img/430161835ae267467687add4a1cdc429_180_0.png)

- 4. 按照提示安装 Photon Minimal OS。

![](img/430161835ae267467687add4a1cdc429_180_1.png)

- 5. 接受所有默认设置。
- 6. 安装操作系统需要几分钟。安装过程中会显示已用计时器。我只用了 96 秒就完成了安装。

![](img/430161835ae267467687add4a1cdc429_181_0.png)

- 7. 安装完成后，按任意键启动系统。

## 配置 Photon

操作系统安装完成后，你需要进行一些安装后配置。

- 1. 系统将重新启动并显示启动画面。操作系统在此期间会进行一些配置，因此你会看到启动画面（参见图 8-3）。

- 5. 输入以下命令重启 SSH 服务：

```bash
systemctl restart sshd
```

- 6. 验证 root 用户能否使用 SSH 登录：

```bash
ssh root@localhost
```

- 7. 输入以下命令退出 SSH 会话：

```bash
exit
```

- 8. 输入以下命令关闭虚拟机：

```bash
init 0
```

- 9. 如果你计划在它上面运行多个 Photon 实例，请编辑设置将其改为 1 个 CPU 和 2 GB 内存。你甚至可以使用 1 GB 内存。

## 在 Photon 上安装 VMware Tools

如果可能，应在所有客户机操作系统上安装 VMware Tools。幸运的是，你可以通过软件包安装 open-vm-tools，而无需自行构建。

- 1. 启动虚拟机，并从 VMRC 或 SSH 以 root 身份登录。
- 2. 输入以下命令安装 open-vm-tools 软件包：

```bash
tdnf -y install open-vm-tools
```

- 3. 输入以下命令启用并启动 VMware Tools：

```bash
systemctl enable vmtoolsd.service
systemctl start vmtoolsd.service
```

- 4. 输入以下命令查看已启动的服务：

  `systemctl status`

- 5. 在输出中，确认显示了 vmtoolsd.service。

![](img/430161835ae267467687add4a1cdc429_184_0.png)

- 6. 在主机客户端中，确认 VMware Tools 正在运行。

![](img/430161835ae267467687add4a1cdc429_185_0.png)

Photon OS 是一款极其轻量级的操作系统。重启虚拟机后，它仅使用了 0.5 GB 内存。很赞的是它有用于安装 VMware Tools 的软件包。它的管理方式与其他 Linux 发行版略有不同，但 Photon OS 网站上有用户和管理员指南。

有趣的事实：包管理器 tdnf 是 Tiny Dandified Yum 的缩写！

## 快照

ESXi 能够为虚拟机拍摄快照。这是一项强大的技术，因为它允许你保留机器在特定时间点的状态和数据。我发现先对虚拟机拍摄快照再进行重大更改（如更新或安装应用程序）非常有用。如果更新成功完成，我会将快照提交到虚拟机中。

我还发现此功能在教学时很有用。在带领学生完成一个流程后，我可以将机器恢复到原始状态，让下一个班级做同样的练习，而无需重新安装操作系统。

一台虚拟机可以有多个快照，无论虚拟机是开机还是关机，你都可以拍摄快照。如果对正在运行的虚拟机拍摄快照，它将捕获其内存和磁盘的当前状态。由于拍摄实时快照需要占用资源，我会对已关机的虚拟机拍摄快照。快照有许多细微之处，我们在此不做讨论，但 VMware 提供了许多优秀的文档来帮助解答相关问题。

## 使用快照

可以从 ESXi 主机客户端启动快照。

- 1. 登录 ESXi 主机客户端。
- 2. 在导航器中，双击**虚拟机**。
- 3. 确认 Fedora 虚拟机已关机。
- 4. 右键单击 Fedora 虚拟机，然后从**快照**下拉菜单中选择**拍摄快照**。

![](img/430161835ae267467687add4a1cdc429_186_0.png)

