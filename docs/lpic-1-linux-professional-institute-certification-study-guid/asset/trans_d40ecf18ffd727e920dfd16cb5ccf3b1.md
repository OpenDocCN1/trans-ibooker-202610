

如果您正在与世界各地的人打交道，使用名为协调世界时（Coordinated Universal Time，UTC）的不同标准通常更容易，是的，该缩写是正确的。UTC是一种不随个人时区而变化的时间（时区在第6章中已介绍）。因此，印第安纳州印第安纳波里斯的UTC时间等于芬兰赫尔辛基的UTC时间。您可以对纽约市的业务伙伴说，“我明天从阿尔及尔、新奥尔良9:44 UTC与您联系”，他们就会知道确切时间，而无需任何特殊计算。（您可以在包括time.is/UTC在内的几个网站上查看当前UTC时间。）

Linux系统通常维护两种类型的时钟。一种基于软件，另一种基于硬件。在这种情况下，硬件时钟也称为实时时钟。即使系统关机，该时钟也会尝试使用系统电池（传统上称为CMOS电池）的电力来保持正确时间。

当系统启动时，Linux操作系统从硬件时钟获取时间并更新其软件时钟。该时钟仅在系统运行期间运行，并被Linux上的许多实用程序使用，这就是为什么它有时被称为系统时间。不幸的是，Linux软件时钟有变得不准确的倾向，尤其是在繁忙的系统上。

## 查看和设置时间

您可以查看（并更改）硬件和软件时钟上的时间，以及在UTC和本地时间之间切换它们。执行此操作的各种实用程序有各自的特殊语法和优点。

### 使用hwclock实用程序

主要用于硬件时钟的命令是hwclock，其语法如下：

```
hwclock [OPTIONS...]
```

hwclock实用程序中常用的OPTIONS在表7.14中描述。

| 短选项 | 长选项 | 描述 |
|---|---|---|
| N/A | --localtime | 将硬件时钟设置为使用本地时间标准 |
| -r | --show | 显示当前硬件时钟时间 |
| -s | --hctosys | 读取当前硬件时钟时间，并将软件时钟设置为该时间 |
| -u | --utc | 将硬件时钟设置为使用UTC标准 |
| -w | --systohc | 读取当前软件时钟时间，并将硬件时钟设置为该时间 |

### 列表7.41: 使用hwclock -r命令读取硬件时钟时间

```
$ hwclock -r
hwclock: Cannot access the Hardware Clock via any known method.
hwclock: Use the --debug option to see the details of our search
for an access method.
$
$ sudo hwclock -r
2019-05-31 14:01:17.622439-0400
$
```

请注意，hwclock实用程序即使只读取时间也需要超级用户权限。还要注意，会显示日期，并且时间以军事格式显示。在某些发行版中，您会看到使用a.m./p.m.格式显示的时间。

> 最好让硬件时钟使用UTC标准。如果将其切换到本地时间标准，可能会出现问题。这些问题可能包括夏令时变更和时区偏移的问题。

### 使用date实用程序

软件时钟主要使用date命令和/或timedatectl实用程序（在下一节中介绍）来查看/设置系统时间。date命令的语法是

```
date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]
```

要查看当前系统时间，请在命令行上输入date。无需特殊权限或选项：

```
$ date
Fri May 31 14:11:23 EDT 2019
```

使用date实用程序设置系统时间需要多花一点功夫。如果您指定的是本地时间，则无需开关，但如果您设置的是UTC，请使用以下选项之一：-u、--utc或--universal。

> 如果您的系统已经在使用网络时间协议（NTP）或简单网络时间协议（SNTP）（本章后面将介绍），则可能无法使用date命令设置软件时钟的时间。您可以通过执行systemctl status ntpd、systemctl status chronyd和systemctl status systemd-timesyncd来检查这些活动服务。如果这些命令中的任何一个显示活动服务，您可能无法使用date命令设置系统时间。

### 列表7.42: 使用date命令更改软件时钟时间

```
# date
Fri May 31 14:26:48 EDT 2019
# date 05301430
Thu May 30 14:30:00 EDT 2019
#
# date
Thu May 30 14:30:02 EDT 2019
#
```

要使用date设置系统时间，必须先指定当前月份，本例中为5月（05）、当前日期，即31日（31），然后使用军事格式指定时间。我们将时间从14:26更改为14:30（1430）。如果需要，您还可以选择指定世纪（CC）、年份（YY）和所需秒数（.ss），如date命令语法中之前所示。

如果您需要修改服务器的时区，本章任务的相关概念在第6章中介绍。

### 使用timedatectl实用程序

timedatectl实用程序是手动管理Linux软件时钟的首选方法。该命令本身不带任何选项即可提供大量信息。（您可以通过添加status选项获取相同信息，但无需这样做。）CentOS发行版上的示例在列表7.43中所示。

### 列表7.43: 在CentOS发行版上使用timedatectl命令

```
$ timedatectl
      Local time: Fri 2019-05-31 15:41:27 EDT
      Universal time: Fri 2019-05-31 19:41:27 UTC
            RTC time: Fri 2019-05-31 19:21:20
        Time zone: America/New_York (EDT, -0400)
    NTP enabled: yes
NTP synchronized: yes
     RTC in local TZ: no
         DST active: yes
   Last DST change: DST began at
                   Sun 2019-03-10 01:59:59 EST
                   Sun 2019-03-10 03:00:00 EDT
Next DST change: DST ends (the clock jumps one hour backwards) at
Sun 2019-11-03 01:59:59 EDT
Sun 2019-11-03 01:00:00 EST
```

该命令的输出可能因发行版而异。列表7.44提供了在Ubuntu发行版上使用timedatectl实用程序显示当前时间信息的示例。

### 列表7.44: 在Ubuntu发行版上使用timedatectl命令

```
$ timedatectl
      Local time: Fri 2019-05-31 15:25:13 EDT
      Universal time: Fri 2019-05-31 19:25:13 UTC
      RTC time: Fri 2019-05-31 19:25:14
      Time zone: America/Indiana/Indianapolis (EDT, -0400)
System clock synchronized: yes
systemd-timesyncd.service active: yes
      RTC in local TZ: no
```

从前两个列表可以看出，系统时钟管理大量信息，包括本地时间和UTC标准下的系统时间、时区以及夏令时。

要更改系统时间，您需要使用timedatectl命令并具备超级用户权限。此外，请按以下语法添加set-time选项和日期/时间：

```
timedatectl set-time "YYYY-MM-DD HH:MM:SS"
```

### 列表7.45: 使用timedatectl命令设置系统时间

```
# date
Fri May 31 15:59:13 EDT 2019
#
# timedatectl set-time "2019-05-31 16:15:00"
Failed to set time: Automatic time synchronization is enabled
#
# timedatectl set-ntp 0
#
# timedatectl set-time "2019-05-31 16:15:00"
#
# date
Fri May 31 16:15:04 EDT 2019
#
```

在列表7.45中，第一次更改系统时间的尝试失败了，因为启用了自动时间同步。要关闭它，请使用timedatectl set-ntp 0命令。您可以稍后通过执行相同的命令重新启用它，但将0更改为1。

如果您正在跟随书中的命令操作，并且通过更改系统上的软件时钟给自己造成了问题，通常可以通过重新启动来修复。只需在命令行输入reboot，然后按Enter键。

您还可以使用timedatectl实用程序管理硬件时钟上的几个项目。例如，您可以在本地时间和UTC标准之间切换时钟。要实现此操作的语法是

```
timedatectl set-local-rtc Boolean-value
```

要强制硬件时钟使用本地时间标准，请将Boolean-value设置为1。要将其更改为UTC标准，请将Boolean-value切换为0。

## 了解网络时间协议

网络时间协议（NTP）是一种网络协议，用于通过网络同步时钟以提供准确的时间。时钟可以位于个人计算机系统、网络路由器、服务器等。实现NTP的程序通常可以同时充当客户端和服务器，并且可以执行对等连接。

为了提供准确的时间，NTP使用一种称为时钟层级方案的机制，该机制提供了一种分层的方法来访问正确的时间源。层级编号从0到15。层级0的设备是高度精确的计时硬件设备，例如原子钟。下一级是层级1，由直接连接到层级0设备的计算机组成。继续沿着层级配置向下，层级2服务器使用网络时间协议客户端软件，允许它们请求由层级1计算机提供的时间数据。

每个层级可能有数千个NTP客户端，每个客户端都从更高层级的NTP服务器或彼此（称为NTP对等体）接收时间更新。图7.3提供了该概念的示意图。

层级0设备具有最准确的时间，直接连接的层级1计算机是下一个最精确的。在这两个层级之后，请求的时间更新作为数据包在网络中传输。由于数据包的网络传输时间可能需要几毫秒（或更久），因此层级配置中较低的服务器的时间准确性会降低。

