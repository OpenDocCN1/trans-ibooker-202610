## Drop-in Directories

在很多不同的上下文中都能看到的一种习惯用法就是一个目录，您可以向其中添加或删除文件以实现某种效果。这对于使用 RPM（Red Hat Package Manager）或 Debian APT（Advanced Package Tool）等包管理系统 Linux 发行版来说非常方便，因为包可以在安装时添加文件或在卸载时删除文件，而无需以编程方式编辑文件。/etc/cron.d/ 就是一个完美的例子；包可以在安装时轻松添加 cron 作业，并在卸载时删除这些作业，而不影响其他任何东西。

如果支持，我们非常喜欢这种技术用于配置文件。例如，您可以有一个 /etc/syslog-ng/syslog-ng.conf 文件，其中包含 @include "/etc/syslog-ng/syslog-ng.local.d"，然后您可以将文件放入 /etc/syslog-ng/syslog-ng.local.d/ 以根据特定节点的需求调整配置。

您也可以在 bash 代码中非常容易地做到这一点（示例 9-9）。

Example 9-9. Include drop-in files

```
# Source local overrides IF any exist AND are executable
for config in /etc/myscript.cfg.d/*; do
  # Override files MUST be EXECUTABLE or they will be ignored, which makes
  # it easy to disable them.
  [ -x "$config" ] && source "$config"
  # Or change `-x` to `-r` to use readable instead of executable
done
```

这个想法建立在第 23 页的“Wrapper Scripts”之上，因此您可以为自己或用户构建简单、可扩展且功能强大的工具。您编写一个简单的骨架脚本，其中包含一些逻辑来在目录中查找“action”命令，以及一些胶水代码来将这些命令组合在一起并提供使用或帮助。然后您添加一个新模块或操作模板，您或您的用户只需复制模板并填写详细信息即可轻松添加新功能。
