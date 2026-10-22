

要进一步修改渲染输出的 output.mp3 音频文件的播放速度，它将被传入 ffmpeg 程序，并附带一个指令过滤器：一个 "atempo" 标志。atempo 过滤器会改变文件的节奏或播放速度。当 atempo 设置为 1.0 时，它不会改变播放速度。要将播放速度改为两倍快，将此值设置为 2.0。

调用 ffmpeg 加速播放并将修改后的文件保存为 faster.mp3。我们将指示 ffmpeg 使用 -vn 参数应用此效果。我们还将在命令行语句中添加 -y 参数。这样做会赋予 ffmpeg 覆盖具有相同名称的现有文件的权限。这样，你在运行此转换以收听播放被更改的方式时，就不必每次都手动输入 Yes。-y 命令行开关在我们本项目的最终脚本中也很有用，因为如果已存在同名音频文件，脚本将继续运行。如果存在，我们允许 ffmpeg 直接覆盖该文件。以下是传递给 ffmpeg 程序的完整命令行语法：

```
$ ffmpeg -i output.mp3 -filter:a "atempo=1.5" -vn -y faster.mp3
```

打开转换后的 faster.mp3 文件，收听与原始 output.mp3 文件相比音频播放快了多少。此外，还查看原始 output.mp3 与转换后的 faster.mp3 的文件大小。它应该大约是原始大小的 25%。在将较大文件与我们的设备同步时，这将特别方便。不仅我们会使用更少的带宽，而且传输时间也会更快。

继续尝试不同的 atempo 值，以找到最适合你收听偏好的速度。请注意，atempo 过滤器的范围在 0.5 到 2.0 之间。如果你想让播放速度更快，你需要两次添加 atempo 过滤器。例如，如果你想将播放速度提高四倍，请尝试以下操作：

```
$ ffmpeg -i output.mp3 -filter:a "atempo=2.0,atempo=2.0" -vn -y faster.mp3
```

如果你是超级听众，你可能能够理解以该速度播放的内容。对我的耳朵来说它是难以理解的，所以我将坚持使用我个人在最终脚本中使用的最佳值 atempo=1.5。

## Tagging the Audio

现在我们已经有一个转换后、加速的 faster.mp3 文件，我们可以简单地将新文件保持原样并完成项目。但虽然 faster.mp3 文件在我们将收听它的音频应用上播放得很好，但演示效果不会很有帮助。faster.mp3 文件缺少重要的元数据，如内容的标题和作者。它也没有任何封面艺术，因此我们的音频播放器程序将为我们未能嵌入文件中的缺失封面艺术替换任何占位符艺术。

我们可以通过调用 Python mutagen 第三方库轻松解决这些缺陷。Mutagen 是一个可以调用来操作 MP3 文件中存储的各种元数据的库。根据 MP3 文件创建者想要的全面程度，MP3 元数据可以包括标题、艺术家、专辑、年份、流派、评论和封面艺术等。对于我们的项目需求，我们将主要关注标题、艺术家和封面图像。

为了保持简单和一致，我们将使用单个 JPEG 文件作为转换后 MP3 文件的相同封面图像。对于下载本书代码的读者，我已保存了本书封面的较小 JPEG，保存为 home.jpg，作为我将用于这些示例的图像。

通过通常的 pip3 install 命令安装 Mutagen 库。

```
sudo pip3 install mutagen
```

以下是针对 faster.mp3 文件的简单测试脚本，用于将标题、艺术家/作者和 home.jpg 图像嵌入到 MP3 文件中：

```
pispeak/mp3tagging.py
```

```
①  from mutagen.mp3 import MP3
    from mutagen.easyid3 import EasyID3
```

```
from mutagen.id3 import ID3, APIC, error
```

```
② mp3_file = EasyID3('faster.mp3')
```

```
③ mp3_file["title"] = "Sample Title"
mp3_file["artist"] = "Content Creator"
mp3_file["album"] = "PiSpeak Project"
mp3_file.save()
```

```
④ mp3_file = MP3('faster.mp3', ID3=ID3)
mp3_file.tags.add(APIC(mime='image/jpeg',type=3,desc=u'PiSpeak Project',
    data=open('home.jpg','rb').read()))
mp3_file.save()
```

让我们看看这个脚本做了什么。

① 从 mutagen Python 库导入特定的 MP3 标记功能。

② 使用 EasyID3() 函数将 mp3_file 变量分配给 faster.mp3 文件。此函数允许我们轻松操作 MP3 标签而无需太多麻烦。

③ 将 MP3 标题、艺术家和专辑元数据嵌入到 test.mp3 文件中。我们在此示例中为这些字段分配了占位符文本。在最终脚本中，它们将更改为与从下载中检索到的相应实体相匹配。

④ 将 home.jpg 图像嵌入为 test.mp3 文件的封面艺术。此艺术将在解析和渲染 MP3 文件中嵌入封面艺术的应用程序中显示，例如文件管理器和某些媒体收藏和播放应用程序。

将脚本保存为 mp3tagging.py 并运行它，确保 home.jpg 和 faster.mp3 源文件与脚本位于同一项目目录中：

```
$ python3 mp3tagging.py
```

在文件管理器中查看更新后的 `faster.mp3` 文件。你现在应该看到它以书的封面呈现。尝试多次运行脚本，更改标题和艺术家，并使用不同的 JPEG 文件作为要嵌入到 MP3 文件中的 `home.jpg` 封面。

我们接近完成项目的所有目标。在我们能够将这些编程片段组装成单个完整的 Python 脚本之前，还有一个步骤要完成。

## Move It

最后一个目标是将 faster.mp3 文件以编程方式移动到 Rclone 配置时创建的 GoogleDrive 文件夹中。为此，请调用 Python 内置的 shutil 库来移动和重命名文件。在与 faster.mp3 文件相同的目录中创建 movefile.py Python 脚本，并使用以下代码将文件移动到 GoogleDrive 文件夹中：

```
pispeak/movemp3file.py
① from pathlib import Path
import platform
import shutil
② homedirectory = str(Path.home())
③ if platform.system() == 'Windows':
    syncfolder = '\\GoogleDrive\\'
else:
    syncfolder = '/GoogleDrive/'
④ mp3name = "faster.mp3"
⑤ mp3destination = homedirectory + syncfolder + mp3name
⑥ shutil.move(mp3name, mp3destination)
```

在运行脚本之前，让我们快速回顾一下列表。

- ① 导入用于识别当前工作和主目录路径以及脚本运行的操作系统的必要的 pathlib、platform 和 shutil Python 库。
- ② 将 Path.home() 对象转换为字符串以获取当前用户主目录的文件路径。
- ③ 调用 platform.system() 函数查看运行脚本的操作系统是什么。如果是 Windows，则将目录分隔符更改为反斜杠。否则，在 Mac 和 Linux 的情况下，使它们成为正斜杠以匹配 Unix 文件路径规范。
- ④ 将 mp3name 变量分配给正在移动的文件的名称；在本例中，它是 faster.mp3 文件。
- ⑤ 将 homedirectory + syncfolder 和 mp3name 字符串分配连接成完整的文件路径并将其分配给 mp3destination 变量。
- ⑥ 调用 shutil.move() 函数，传入 mp3name 和 mp3destination，将 MP3 文件移动到用户主目录中的 GoogleDrive 目录。

保存文件并运行 movemp3file.py 脚本：

```
$ python3 movemp3file.py
```

你刚刚完成了项目的最后一部分。现在是时候将这些单独的代码片段组装成一个单一的、连贯的脚本，按顺序执行这些步骤。

在最终项目脚本中，你将添加邮件通知。此电子邮件正文将列出自上次运行脚本以来新文章的标题和链接摘要。它还将指示已编译并传输到 Google Drive 以供下载的新 MP3 文件的名称。最后，脚本将包括删除原始 output.mp3 源文件的说明。这将有助于继续保持脚本的工作目录整洁。当你准备好时，请继续阅读下一部分，其中列出了项目的最终完整脚本。

