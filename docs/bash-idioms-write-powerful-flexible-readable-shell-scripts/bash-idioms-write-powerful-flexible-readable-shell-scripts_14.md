## 这是个陷阱！

trap 内置命令（help trap）允许你在脚本接收到信号时执行代码，其中包括脚本退出时。这是一个很好的地方放置“清理”代码，即使有人 CTRL-C（但不是 kill -9）脚本也能工作。你也可以用它运行调试代码，但对于这种简单情况，只需使用 set -x 或在 bash -x 下运行即可。运行 trap -l 列出系统支持的所有信号，或 trap -p <signal> 显示与该信号关联的 trap 命令。

trap 的典型用途是清理脚本，如前所述。语法是 trap [-lp] [[arg] signal_spec ...]，而 arg 可以是自包含的代码，但最好只是定义一个函数然后调用它会更清晰。这看起来像示例 9-12（输出显示在示例 9-13 中）。

### 示例 9-12. 临时 trap 示例：代码

```
#!/usr/bin/env bash
# trivial_trap.sh: Trivial bash trap example
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch09/trivial_trap.sh
# --------------------------------------------------------------------------------
# Does not work on Zsh 5.4.2!

function Cleanup {
    echo "$FUNCNAME was triggered!  Cleaning up..."
}

echo "Starting script $0..."

echo 'Setting the trap...'
# Will call Cleanup on any of these 6 signals
trap Cleanup ABRT EXIT HUP INT QUIT TERM

echo 'About to exit...'
```

### 示例 9-13. 临时 trap 示例：输出

```
Starting script examples/ch09/trivial_trap.sh...
Setting the trap...
About to exit...
Cleanup was triggered!  Cleaning up...
```

另请参阅 GNU 文档关于 trap 和第 120 页的“bash 调试”。
