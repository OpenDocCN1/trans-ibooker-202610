

7. 输入以下命令，安装编译 VMware Tools 所需的工具：

```bash
apt update
apt install build-essential -y
```

8. 下载 VMware Tools 构建脚本，使其可执行，然后运行它，输入：

```bash
wget https://raw.githubusercontent.com/FentonTom/VMware_Tools_Ubuntu/main/Ubuntu2004
chmod 755 Ubuntu2004
./Ubuntu2004
```

9. 当 VMware Tools 编译并安装完成后，您将收到服务已启动的消息。

![](img/430161835ae267467687add4a1cdc429_154_0.png)

10. 返回 ESXi 主机客户端，选择 Ubuntu 虚拟机。它将显示 VMware Tools 已安装并在其上运行。您可能需要刷新屏幕。

![](img/430161835ae267467687add4a1cdc429_155_0.png)

## Ubuntu 虚拟机的后续配置

除非您只打算在 ESXi 主机上运行单个虚拟机，否则您需要减少虚拟机使用的资源。

1. 关闭虚拟机电源。这可以通过虚拟机、VMRC 或主机客户端完成。
2. 在 ESXi 主机客户端中，右键单击虚拟机并选择 **编辑设置**。

![](img/430161835ae267467687add4a1cdc429_155_1.png)

3. 将 vCPU 数量更改为 1，RAM 更改为 2GB，删除 CD/DVD，然后点击 **保存**。
4. 从 VMRC 打开虚拟机电源。
5. 验证系统已启动，然后将其关闭。

## 虚拟机文件

虚拟机是文件的集合。以下列表中显示的文件包含虚拟机监控程序运行虚拟机所需的数据和信息。

```text
[root@ESXARM01:/vmfs/volumes/5f4e5cd6-2dc632aa-9320-dca632cdd252/Fedora34_01_Argon] ls -l
total 33561728
-rw-r--r-- 1 root root 159286 Sep 13 2020 vmware.log
-rw-r--r-- 1 root root 91597 Sep 13 2020 vmware-4.log
-rw-r--r-- 1 root root 105961 Sep 13 2020 vmware-3.log
-rw-r--r-- 1 root root 87922 Sep 13 2020 vmware-2.log
-rw-r--r-- 1 root root 86790 Sep 13 2020 vmware-1.log
-rwxr-xr-x 1 root root 3217 Sep 13 2020 Fresh_Fedora34_NFS.vmx
-rw-r--r-- 1 root root 0 Sep 13 2020 Fresh_Fedora34_NFS.vmsd
-rw------- 1 root root 479 Sep 13 2020 Fresh_Fedora34_NFS.vmdk
-rw------- 1 root root 278840 Sep 13 2020 Fresh_Fedora34_NFS.nvram
-rw------- 1 root root 34359738368 Sep 13 2020 Fresh_Fedora34_NFS-flat.vmdk
```

以下是一些文件及其用途：

- **nvram** — 包含虚拟机 BIOS 的状态。
- **vmdk** — 存储虚拟机虚拟磁盘的内容。一个虚拟机可以有多个虚拟磁盘。
- **vmsd** — 存储虚拟机快照的元数据。
- **vmx** — 虚拟机的配置文件。它存储虚拟机设置。
- **log** — 包含日志。

这些文件可以通过主机客户端查看。

1. 在主机客户端中，展开“存储”对象并选择 rPi-USB-Datastore-01。

![](img/430161835ae267467687add4a1cdc429_157_0.png)

2. 选择“数据存储浏览器”。

![](img/430161835ae267467687add4a1cdc429_157_1.png)

3. 选择虚拟机。文件将显示在右侧。

![](img/430161835ae267467687add4a1cdc429_158_0.png)

## 创建虚拟机副本

创建虚拟机后，您可能需要它的另一个实例。VMware 可以在 vSphere Client 中使用克隆功能通过 GUI 轻松创建虚拟机副本，但由于我们尚未安装 vCenter Server，只有 ESXi 主机客户端可用，因此我们需要使用 ESXi 主机客户端来完成。确实可以做到，但使用它复制虚拟机比使用 vCenter Server 需要更多的步骤。

## 复制虚拟机文件

组成虚拟机的文件存储在可从 ESXi 访问的目录中。您可以复制这些文件来创建重复的虚拟机。

1. 在到 ESXi 主机的 SSH 会话中，切换到包含虚拟机的目录：

```bash
cd /volumes/rPi-USB-Datastore-01
```

4. 复制过程完成后，我有了一个新目录，其中包含虚拟机的文件。

![](img/430161835ae267467687add4a1cdc429_160_0.png)

## 注册复制的虚拟机

复制虚拟机的文件后，您需要让 ESXi 主机感知到它。这通过注册来完成。

1. 在主机客户端中，展开“存储”对象，然后点击您复制文件到的数据存储。
2. 选择“注册虚拟机”。

![](img/430161835ae267467687add4a1cdc429_160_1.png)

3. 选择 **Fresh_Ubuntu2004** 目录，右键单击 `.vmx` 文件，然后选择 **注册虚拟机**。

![](img/430161835ae267467687add4a1cdc429_161_0.png)

4. 关闭数据存储浏览器。

5. 在导航器中选择 **虚拟机**。右键单击底部名为 Ubuntu2004_A 的虚拟机，然后选择 **编辑设置**。

![](img/430161835ae267467687add4a1cdc429_161_1.png)

6. 选择 **虚拟机选项**。
7. 验证其目录显示为 Fresh_Ubuntu2004；如果不是，请选择另一个虚拟机。
8. 在 **虚拟机配置文件** 文本框中，输入 Fresh_Ubuntu2004_A，然后点击 **保存**。

![](img/430161835ae267467687add4a1cdc429_162_0.png)

9. 点击 **刷新**，验证新虚拟机的名称是否显示。

![](img/430161835ae267467687add4a1cdc429_162_1.png)

## 启动操作系统

1. 打开新虚拟机的 VMRC 控制台并打开其电源。
2. 在弹出的窗口中，选择 **我复制了它**，然后点击 **应答**。
3. 登录 Ubuntu 系统。
4. 输入以下命令获取 root 权限：

```bash
sudo bash
```

5. 输入以下命令，验证其 IP 地址与复制来源的虚拟机不同：

```bash
ip add
```

6. 输入以下命令，验证其名称与复制来源的虚拟机相同：

```bash
hostname
```

![](img/430161835ae267467687add4a1cdc429_163_0.png)

我将安装 Fedora 34，但已知许多其他版本也可以与 ESXi on Arm 配合使用。

创建 Fedora 虚拟机的过程与安装 Ubuntu 以及许多其他 Linux 发行版非常相似。

Arm Fedora 镜像的下载站点是 https://getfedora.org/en/server/download/。

我将直接从该网站下载 *Fedora 34：适用于 aarch64 的标准 ISO 镜像* 到 ESXi 数据存储。

![](img/430161835ae267467687add4a1cdc429_165_0.png)

## 下载 Fedora 34 ISO

我将把安装 ISO 直接下载到 ESXi 数据存储。

1. SSH 进入 ESXi 系统。
2. 输入以下命令获取 root 用户权限：

```bash
sudo bash
```

3. 在“新建虚拟机”向导中，选择“创建新的虚拟机”，然后点击“下一步”。

![](img/430161835ae267467687add4a1cdc429_167_0.png)

4. 在“名称”文本框中，输入 Fedora34_01。从“客户机操作系统系列”下拉菜单中，选择 Linux。在“客户机操作系统版本”下拉菜单中，选择 Red Hat Fedora (64-bit)，然后点击“下一步”。

![](img/430161835ae267467687add4a1cdc429_167_1.png)

# 第 8 章    ESXi 上的虚拟机

5. 验证 rPi-USB-Datastore-01 是数据存储，然后点击 **下一步**。
6. 将虚拟硬件设置为三个 CPU、4GB 内存和 32GB 硬盘。
7. 展开 CD/DVD **驱动器 1**，选择 **数据存储 ISO 文件**，然后选择您的 Fedora ISO 镜像。验证已选择 **打开电源时连接**。
8. 展开 **硬盘 1**，然后选择 **精简置备**。点击 **下一步**。

![](img/430161835ae267467687add4a1cdc429_168_0.png)

9. 验证您的设置，然后点击 **完成**。

## 在虚拟机上安装 Fedora 34

在虚拟机上安装操作系统就像在物理机上安装一样。我将使用之前安装的 VMRC 来创建 Fedora 虚拟机。

1. 右键单击 Fedora 虚拟机，然后选择“启动远程控制台”。

![](img/430161835ae267467687add4a1cdc429_169_0.png)

2. 在 VMRC 顶部，点击“打开电源”图标。
3. 在显示界面中，选择“安装 Fedora 34”。

![](img/430161835ae267467687add4a1cdc429_169_1.png)

![](img/430161835ae267467687add4a1cdc429_171_0.png)

6. 安装完成后，选择“立即重启”。

## 配置 Fedora 34

操作系统安装完成后，您需要进行一些安装后配置。

1. 系统将重新启动到登录提示符。
2. 虽然我可以从命令提示符登录系统，但我使用 Web 门户与系统交互。Web 控制台的 IP 地址显示在 VMRC 中。

