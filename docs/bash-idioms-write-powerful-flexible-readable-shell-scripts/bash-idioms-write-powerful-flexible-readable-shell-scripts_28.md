## Dumping Your Environment

当从 cron、持续集成或其他环境可能发生变化或与你期望的不同之处运行时，记录或转储你的环境通常很有帮助。你会看到外部 env 或 printenv 用于此目的，但 bash 内部的 set 命令会给你更多信息，包括函数。另一方面，它嘈杂得多，所以全部检查一下并决定哪个最适合你。

![](img/553ffa1bc15093729b691f706befc0ac_137_0.png)

## Environment Information Leaks

如果你记录或转储你的环境，请注意它通常会包含敏感信息，例如用户名、密码、API 密钥等等！你可能希望修改你的转储命令以删除数据，也许使用 sed 或 Perl 单行命令：

```
set | perl -pe 's/^(SECRET=).*\/1<REDACTED>/g;'
```

从 bash v3.0 开始，有一个 --debugger 标志和 shopts extdebug 选项，但在这里我们必须诚实地说我们从未使用过这些选项；set -x 一直足够。运行 bash --debugger -- /path/to/script 很可能会导致：

/path/to/script: /usr/share/bashdb/bashdb-main.inc: No such file or directory
/path/to/script: warning: cannot start debugger; debugging mode disabled

如果你看到这个，你需要查看 Bash Debugger Project，它是“一个遵循 gdb 命令语法的 bash 源代码调试器”。它是 bash 版本相关的，所以确保为你的系统获取正确的版本。

另请参阅：

- - http://bashdb.sourceforge.net
- - Example 10-3
- - “bash Linter”第 134 页
- - “Debug and Verbose”第 82 页
- - “printf for Reuse or Debugging”第 58 页
- - “Unofficial bash Strict Mode”第 96 页
