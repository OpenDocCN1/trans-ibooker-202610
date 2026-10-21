## 其他

其他指南：

- 对于“system”脚本，记录到 syslog，让操作系统处理最终目的地、日志轮转等。
- 错误信息应发送到 STDERR，如 echo 'A Bad Thing happened' 1>&2。
- 使用 [ -x /path/to/tool ] || { ...error code block... } 来检查外部工具是否可用。
- 失败时提供有用的信息。
- 设置退出代码，尤其是在失败时。
