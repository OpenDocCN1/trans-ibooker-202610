## 我们是交互式的吗？

有时你的代码需要知道它是否在交互式会话中运行。它可能需要知道这一点，因为它会改变行为，比如请求输入，或者你可能只想为交互式 shell 设置某些配置选项。

GNU 文档关于交互式 shell 说要使用以下其中一种，因此你会经常看到这些或其变体：

```
case "$ -" in
  *i*) echo This shell is interactive ;;
  *)   echo This shell is not interactive ;;
esac
```

或：

```
if [ -z "$PS1" ]; then
  echo This shell is not interactive
else
  echo This shell is interactive
fi
```

你也可以考虑 -t FD 测试，如果文件描述符打开在终端上则为真。如果未指定文件描述符，则使用 0（STDIN）。你可以将其与其他测试结合，比如 bash 本身，例如：

```
# Only if bash in a terminal!
if [ -t 1 -a -n "$BASH_VERSION" ]; then
  echo 'bash in a terminal'
else
  echo 'NOT bash in a terminal'
fi
```



or:

```
# Only if *interactive* bash in a terminal!
if [ -t 1 -a -n "$PS1" -a -n "SBASH_VERSION" ]; then
  echo 'Interactive bash in a terminal'
else
  echo 'NOT interactive bash in a terminal'
fi
```

注意我们故意使用了 []（test）而不是 [[]]（bash 条件），因为其要点之一是此代码可能在非 bash 环境中运行，而该环境不支持 [[]]（例如 dash）。

## Summary

本章介绍的是那些并不漂亮但你终究需要知道的惯用法。有很多种方法可以在 bash 中处理文件，通常涉及重定向和/或管道，因此我们展示了一些典型的 bash 模式。Bash 脚本本身也是文件，因此我们谈到了将文件变成可执行脚本或库的魔法咒语，如何退出并清理，以及其他有用的惯用法。虽然“here-documents”并不完全是文件，但它们经常取代单独的文本文件，或文件中的文件，我们在别处使用过它们，所以这里也有。
