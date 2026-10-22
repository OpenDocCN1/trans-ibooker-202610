### 第 3 章

#### Twitch 计时器

将 Raspberry Pi 改造成媒体中心是其更受欢迎的用途之一。Pi 优化的操作系统发行版，例如 LibreELEC[^29] 和 OSMC（开源媒体中心），[^30] 为 Pi 提供了一种有吸引力且易于导航的方式来访问开源和商业媒体服务，例如 YouTube、Netflix 和 Twitch。如果你只想用 Pi 来消费流媒体内容，这些选项做得非常出色。但如果你希望 Pi 不仅用于媒体消费，并且对硬件上运行的内容有完全控制权，本项目展示了一种解决此问题的方法。

我喜欢在漫长的工作日后观看我最喜欢的 Twitch 主播，通常在床上安顿下来观看直播。但太多时候我半夜醒来时显示器仍开着，Twitch 未观看的直播仍在消耗带宽。我们将在第 9 章 *语音红外控制* 中讨论一种使用 Pi 关闭电视的方法，因此本项目将完全专注于如何使用 Pi 连接到 Twitch，将播放定向到流行的 VLC 播放器，然后在经过一定时间后关闭流媒体。我们将仅用几行简短的 Python 代码完成所有这些。





##### 设置

以下是构建此项目所需的组件。

###### 硬件

- HDTV 显示器，或带 HDMI 输入和内置扬声器的计算机显示器

###### 软件

- VLC 媒体播放器（已预装在 Raspberry Pi OS Desktop 发行版中）
- Streamlink[31]

VLC 媒体播放器随 Raspberry Pi OS Desktop 预装。VLC 是最受欢迎的开源媒体播放器之一，这得益于其能够播放大量不同的媒体格式。其中一种媒体格式就是 Twitch 用于流式传输其内容的 文件类型。

在 Pi 的 Sound & Video 桌面菜单中定位 VLC 图标（它看起来像一个橙白条纹的安全锥），并启动该应用程序。现在，它只是一个黑屏，中央有一个更大的应用程序图标，看起来并不有趣。让我们改变一下。关闭应用程序并跟随操作。

我们将使用 Streamlink 包来完成此项目。但在这样做之前，让我们先创建一个 projects 文件夹来存储本书的各种 Python 项目脚本。打开一个终端窗口并输入 mkdir 命令：

```
$ mkdir ~/projects/
```

然后，使用 cd 命令切换到该目录，以便您可以克隆一个 Git 仓库到其中：

```
$ cd ~/projects/
```

回想一下上一章，您已经提前为此项目创建了一个 Git 仓库。现在连接到并与该 Git 仓库同步：

```
$ git clone ~/repository/twitchtimer.git
```

git clone 命令将在 projects 文件夹内创建一个名为 twitchtimer 的新文件夹。使用 cd 命令切换到 twitchtimer 文件夹：

```
$ cd ~/projects/twitchtimer
```

现在让我们使用 touch 命令创建初始 Python 脚本文件，并将该初始空文件提交到仓库：

```
$ touch twitchtimer.py
$ git add twitchtimer.py
$ git commit -m "Initial TwitchTimer Project commit."
$ git push
```

touch 命令用于创建空文件，在本例中为 twitchtimer.py。这将是我们用于保存此项目 Python 代码的文件。下一个命令 git add 将 twitchtimer 文件夹中从 Git 仓库克隆的本地 Git 存储中的文件添加到我们的本地 Git 存储中。命令 git commit 加上额外的 -m "Initial TwitchTimer Project commit." 参数会提交并永久存储自上次运行 commit 命令以来对该文件所做的更改。-m 表示我们希望与提交一起包含的可选消息。当回顾特定文件或一组文件的先前提交时，这些消息会很有帮助。

最后，git push 命令默认尝试将我们提交的更改推送到我们克隆的原始 Git 仓库。在本例中，它将 twitchtimer 目录中我们所做的更改复制到 ~/repository/twitchtimer.git Git 仓库。我们将在每次对 twitchtimer.py 文件进行重要或值得注意的更改时重新运行这些 git 命令。我们将对本书中的每个后续项目使用这种相同的循环 git 方法。

接下来，使用 pip3 在终端窗口中安装基于 Python 的 Streamlink 应用程序：

```
$ sudo pip3 install streamlink
```

运行此命令将安装 Streamlink 以及其正常运行所依赖的额外第三方 Python 库。您可以通过在终端中请求其版本来检查已安装的 Streamlink 版本：

```
$ streamlink --version
streamlink 2.0.0
```

Streamlink 是一个功能强大且灵活的命令行实用程序，它使用户能够将 Twitch 和 YouTube 等使用的媒体流重定向到 VLC 等媒体播放器。由于其灵活性，可以传递给 streamlink 命令的参数数量令人望而生畏。您可以通过阅读程序的用户指南来了解更多关于可以伴随 streamlink 命令的各种参数的信息。[32] 虽然可以使用的参数 literally 有几十个，但我们将专注于此项目的特定 Twitch 参数。

为了验证 VLC 和 Streamlink 是否已成功安装，请连接到全天 24 小时广播的 Twitch 流。其中之一是 MST3K Twitch 频道。要确定 Twitch 频道的名称，请启动网络浏览器并导航到 twitch.com。在 Twitch 主页的搜索框中输入 mst3k。搜索结果应显示 MST3K 在页面顶部。选择 MST3K 并注意 URL，现在显示为 twitch.tv/mst3k。因此，mst3k 是 MST3K 频道的名称。尝试几次其他搜索以熟悉这种方式识别频道名称。

现在我们知道了 MST3K 在 Twitch 上的频道名称，我们可以将它作为命令行参数传递给 Streamlink，同时指定视频流播放质量：

```
$ streamlink twitch.tv/mst3k 720p
```

如果一切安装和配置正确，VLC 播放器将启动并播放 MST3K 的 Twitch 直播。

请注意，并非所有 Twitch 流都以 720p 广播，而其他一些流可能会选择以每秒六十帧的速度广播其 720p 流（720p60）。要验证流使用的可用格式和分辨率，请在不指定流质量的情况下运行 streamlink 命令。以 MST3K 流为例，这样做会产生以下结果：

```
$ streamlink twitch.tv/mst3k
```

```
Available streams: audio_only, 160p (worst), 360p, 480p, 720p, 1080p (best)
```

根据您拥有的 Pi 和 Internet 连接的带宽，您可能会看到流播放质量偶尔出现卡顿。如果是这样，请尝试将流质量从 720p 分辨率降低到 480p：

```
$ streamlink twitch.tv/mst3k 480p
```

MST3K 最初是在 90 年代播出的，早于 HDTV 的出现，因此 480p 与更高分辨率相比看起来不会有任何不同。除了减轻 Pi 中央处理器和图形处理器的负载外，480p 还会消耗您网络上的更少带宽。

现在我们已经在 VLC 中实现了 Twitch 播放，让我们编写一个简单的 Python 脚本，该脚本将自动启动此流，并在经过一定时间后将其关闭。

##### 创建睡眠计时器脚本

如果您安装了 Visual Studio Code，您可以使用该编辑器打开我们之前创建的空 twitchtimer.py 文件。但鉴于此项目 Python 脚本的简短性，现在使用 Nano 文本编辑器可能更容易：

```
$ nano twitchtimer.py
```

手动输入或复制并粘贴以下 Python 代码到编辑器窗口中：

```
twitchtimer/twitchtest.py

① import os, subprocess, signal, sys, time
② duration = int(sys.argv[1])*60
③ cmd = "streamlink twitch.tv/mst3k 720p"
④ twitchstream = subprocess.Popen(cmd,shell=True)
⑤ time.sleep(duration)
⑥ os.killpg(os.getpgid(twitchstream.pid), signal.SIGTERM)
```

① 代码的第一行导入了我们在脚本后面部分将依赖的 Python 标准库。

② 我们将在运行脚本时作为参数传递一个数值。此整数将指示我们希望在强制流退出之前流式传输的分钟数。Python 的 time.sleep() 方法将暂停脚本执行，暂停的时间为分配给它的秒数。由于传入的参数是分钟，因此我们需要通过乘以 60 将该值转换为秒。

③ 我们将创建一个名为 cmd 的字符串变量，用于存储我们希望 Python 脚本执行的命令。在本例中，它是我们用于初始测试 Streamlink 安装时使用的相同命令。

④ 我们将在 Python 脚本中创建一个子进程，该子进程将执行 streamlink 命令，连接到 MST3K 直播，并打开 VLC 媒体播放器应用程序以查看流。

⑤ 我们将指示 Python 脚本暂停对任何剩余脚本的进一步执行，直到我们传递给脚本参数的分钟数过去。

⑥ 一旦 duration 变量中存储的秒数过去，我们将告诉 Python 脚本杀死它在启动 streamlink 命令时创建的进程。这包括它创建的任何子进程，例如退出已启动的任何 VLC 播放器。

保存文件并返回到终端窗口。运行脚本并传递数字 2，如下所示：

```
$ python3 twitchtimer.py 2
```

数字 2 将乘以 60，并将结果 120 秒存储到脚本的 duration 变量中。这就是脚本在启动 Streamlink 应用后等待的时间。两分钟（120 秒）过去后，脚本将杀死 Streamlink 进程以及正在播放流的 VLC 程序：

```
$ python3 twitchtimer.py 2
```

成功测试后，将您最近保存的 twitchtimer.py Python 脚本提交到您的 Git 仓库：

```
git commit -m "Initial Python script using streamlink process call."
```




##### TwitchTimer 脚本改进

我们基本上已经完成了项目，因为我们已经成功连接到一个广播中的 Twitch 频道，打开了 VLC，并在设定的持续时间后关闭了 VLC 和流。但我们的脚本并不够健壮。例如，尝试在不传递数值（甚至完全不传递值）的情况下运行脚本。它将无法运行，具体报错为 ValueError 或 IndexError，取决于作为参数传递了什么或未传递什么。

此外，虽然 MST3K 在其 Twitch 频道上提供持续的娱乐，但它并不是 Twitch 上唯一的广播者。如果我们能向脚本传递另一个参数，指定我们想要观看哪个频道以及观看多长时间，那就很方便了。幸运的是，我们并不是唯一需要在 Python 脚本中进行健壮命令行处理的程序员。argparse[33] 是一个内置的 Python 库，我们可以将其导入到 twitchtimer.py 脚本中，以帮助解析脚本的命令行参数。我们还可以用它来告知用户脚本需要什么样的参数才能执行脚本，并通过有用的说明和示例来执行脚本。这在几个月后你写了这样的脚本但忘记了脚本需要哪些参数才能正确执行时将会很有帮助。

让我们修改原始脚本以包含 argparse 库，并将其应用于要传递的 duration 和 channel 参数。我们还将包含一些基本的错误处理，以更优雅地处理可能出现的任何异常。最后，作为奖励，我们将根据脚本执行的系统不同而以不同的方式终止进程：

```
twitchtimer/twitchtimer.py

import argparse, os, platform, signal, subprocess, sys, time

① parser = argparse.ArgumentParser()
parser.add_argument('-c', action='store', dest='channel',
help='Twitch name of channel to stream', required=True)
```

```
parser.add_argument('-t', action='store', dest='duration',
help='Number of minutes you wish to stream', required=True, type=int)

args = parser.parse_args()

duration = int(args.duration)*60

② try:
    cmd = "streamlink twitch.tv/" + args.channel + " 720p"
    twitchstream = subprocess.Popen(cmd,shell=True, stdout=subprocess.PIPE)

③ while True:
    line = str(twitchstream.stdout.readline())
    if not line:
        break
    if line.__contains__('error: Unable to validate '):
        print('Unable to connect to channel, either because \
channel is not broadcasting now or channel name is \
invalid. Quitting.')
        quit()
    if line.__contains__('Opening stream'):
        break

print('Stream ' + args.channel + ' for ' + str(duration) + ' seconds.')

④ time.sleep(duration)

⑤ if platform.system() == 'Windows':
    os.system("taskkill /IM vlc.exe")
else:
    os.killpg(os.getpgid(twitchstream.pid), signal.SIGTERM)

print('Streamed ' + args.channel + ' for ' + \
str(args.duration) + ' minutes.')

⑥ twitchstream.kill()
time.sleep(5)
os._exit(-1)
time.sleep(5)
sys.exit(-1)

finally:
    print('Stream closed.')
```

① 在导入 argparse 库后，我们可以利用它不仅解析传递给脚本的命令行参数，还可以添加一些帮助文本，甚至进行简单的验证以确认我们传递的变量类型。为了消除传递值位置上的任何歧义，我们将要求频道前面加上 -c 标志，播放时间加上 -t 标志。由于我们无法在没有这两个值的情况下处理脚本，我们将使它们成为必需项。这样，如果命令行中缺少一个或两个值，我们的脚本将优雅地失败，并向用户提供说明，告知其需要哪些值才能继续。

② 每当脚本执行例程或，如我们案例中那样，执行可能产生意外或非计划结果的外部程序时，最好将此类例程包装在 try/catch/finally 块中。如果发生意外错误，脚本可以尝试从错误中恢复。我们的脚本中可能发生多种错误，例如断管（数据流意外丢失）。通常我们会尝试识别每种错误条件，并指示脚本通过需要执行的任何纠正代码从这些错误中恢复。但出于时间和篇幅的考虑，我们在全面方法上做了简化，转而使用 finally 关键字作为万一发生错误时的兜底处理。在这种情况下，当我们终止 VLC 播放器并结束流时，如果我们没有正确等待子进程优雅退出，我们可以在终端中打印出 Stream ended. 消息。

③ While True 循环检查我们传递给 streamlink 的频道名称是否是一个有效且当前正在广播的频道。如果 streamlink 无法连接到请求的频道的流，我们将优雅地将结果打印到终端。如果 streamlink 能够连接，从而报告它正在 Opening stream，我们将退出循环并继续执行脚本的其余部分。

④ 就像我们的原始脚本一样，我们暂停脚本的进一步执行，持续时间为命令行参数中传递的 duration。

⑤ 这个完全可选的条件检查我们是否在 Windows 上执行此脚本。如果是，Windows 处理进程终止的方式与 Linux 不同。我之所以包含此检查，仅仅是因为这是唯一一个不依赖于 Pi 特定硬件来执行的项目。Python 可移植是有原因的，而这个脚本体现了 Python 在多个操作系统和硬件平台上运行的能力。如果你想在 Windows 笔记本电脑上运行此脚本，例如，只要在上面安装了 Streamlink 和 VLC 播放器，你就可以这样做。

⑥ 添加几个额外的计时器和系统调用，以应对在我们终止 VLC 播放器时意外关闭的 Streamlink 子进程数量。如果你通过 # 字符注释掉这些命令并运行脚本，你可能会在脚本终止时在终端中看到一连串错误。time.sleep(5) 函数在调用 os._exit(-1) 和 sys.exit(-1) 函数之前等待五秒钟。这样做实现了优雅退出流并关闭播放器的目标。

尝试运行脚本，显式传递 -c 和 -t 值。例如，如果我们想让 MST3K 频道作为测试播放两分钟，请在终端窗口中执行以下操作：

```
$ python3 twitchtimer.py -c mst3k -t 2
```

只要脚本正确输入并格式化，且缩进正确，你应该会看到 VLC 播放器出现并播放流两分钟，然后退出播放器并在终端窗口中返回播放频道和持续时间。为了演示此脚本的可移植性，这里有一个在 Windows 计算机上成功结束播放会话后终端窗口输出的示例：

```
python3 .\twitch.py -c mst3k -t 2
```

Stream mst3k for 120 seconds.
SUCCESS: Sent termination signal to the process "vlc.exe" with PID 15920.
Streamed mst3k for 2 minutes.

尝试不同的频道和持续时间组合，看看脚本如何处理它们。尝试省略任一值或标志以测试参数错误处理。输入一个错误的频道名称，看看它如何从“streamlink unable to connect to the stream”中恢复。此外，如果脚本似乎挂起，请确保流分辨率值设置为频道支持的分辨率，正如本章前面提到的。

一旦你对脚本的修改感到满意，请记住将更改提交并推送到你的 Git 仓库：

```
$ git add .
```

```
$ git commit -m "Improved argument handler and exception handling."
```

```
$ git push
```

恭喜你完成了书中的第一个项目。为了最大限度地减少编辑冗余，未来的项目将不会明确审查为这些项目设置 Git 的步骤。

##### 后续步骤

Twitch Timer 展示了将你的 Pi 变成具有可编程睡眠定时器的流媒体播放设备是多么容易。现在你可以安心入睡，而不用担心几个小时后醒来发现流未被观看以及不必要浪费的数 GB 带宽。

在完成第 9 章 *Voice IR Control* 中的后续项目后，你可以添加关闭你观看流的电视显示器的能力，前提是电视使用红外（IR）遥控器来打开或关闭电视。你也可以在设定的持续时间后停止请求的 Twitch 流。

你也可以通过对 Discord[^34] 或电子邮件通知做出反应来启动 Twitch Timer，提醒你你最喜欢的 Twitch 主播正在直播。但现在，请坐下来，享受你在本章中取得的进展，通过观看你喜欢的 Twitch 频道的几分钟内容。

##### 脚注

+   [29] https://libreelec.tv/
+   [30] https://osmc.tv/
+   [31] https://streamlink.github.io/
+   [32] https://streamlink.github.io/
+   [33] https://docs.python.org/3/library/argparse.html
+   [34] https://discord.com/




