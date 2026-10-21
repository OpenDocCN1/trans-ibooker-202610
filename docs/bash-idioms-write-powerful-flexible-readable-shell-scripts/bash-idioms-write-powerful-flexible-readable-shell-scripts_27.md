## bash Debugging

我们已经在“Debug and Verbose”第 82 页讨论过在代码中添加调试语句、“printf for Reuse or Debugging”第 58 页和示例 10-3 中显示调试值，但我们直到现在才谈到解释器级别的调试。

首先，你可以通过运行 bash -n /path/to/script 来进行粗略的语法检查。技术上，bash -n 表示“读取命令但不执行它们”（参见 help set），它类似于 perl -c。这不会告诉你运行时错误、错误逻辑、外部命令的错误选项等。但它会告诉你引号、括号和方括号不平衡；bash 内置的无效语法；等等。这是在编写或更改脚本时早期和经常运行的一个很棒的命令。

有一个类似的 bash -v 或 set -v 命令，表示“打印 shell 输入行，因为它们被读取”，但实际上它没有听起来那么有用，因为它在 shell 插值之前显示，所以你只是得到源代码被吐回给你。

bash -x（xtrace），另一方面，表示“打印命令及其参数，因为它们被执行”，非常有用，这是调试的下一步。运行 bash -x /path/to/script，并准备好面对大量输出。参见示例 10-3，了解使该输出更有用的方法之一。你也可以在代码中间使用 set -x 打开 xtrace，然后稍后使用 set +x 关闭它（是的，这也让我们头疼了一点，但一旦你使用 - 打开它，使用 + 关闭它至少是可记忆的）。示例 10-20 说明了这一点。

### Example 10-20. Simple debug example

```
### Without debugging
$ examples/ch10/select-ssh.sh
1) Exit          3) gitlab.com      5) mythtv-be01
2) git.atlas.oreilly.com  4) github.com      6) kitchen
SSH to> 1
```

```
### With debugging (2 lone lines broken to fit)
$ bash -x examples/ch10/select-ssh.sh
+xtrace examples/ch10/select-ssh.sh:9:: ssh_config=examples/ch10/ssh_config
+xtrace examples/ch10/select-ssh.sh:11:: PS3='SSH to> '
+xtrace examples/ch10/select-ssh.sh:12:: Select ssh_target in Exit $(egrep \
-i 'Host \w+' "$ssh_config" | cut -d ' ' -f2-)
++xtrace examples/ch10/select-ssh.sh:12:: egrep -i '^Host \w+' \
examples/ch10/ssh_config
++xtrace examples/ch10/select-ssh.sh:12:: cut '-d ' -f2-
1) Exit          3) gitlab.com      5) mythtv-be01
2) git.atlas.oreilly.com  4) github.com      6) kitchen
SSH to> 1
+xtrace examples/ch10/select-ssh.sh:13:: case $REPLY in
+xtrace examples/ch10/select-ssh.sh:14:: exit 0
```

你可能想看看第 96 页的“Unofficial bash Strict Mode”，它可以在稍后添加时揭露微妙问题，或在编写时防止它们出现。

![](img/553ffa1bc15093729b691f706befc0ac_136_0.png)
