## Shebang!

是什么让一个普通的旧文件成为 shell 脚本？Shebang！

我们认为“shebang”听起来像是一个咒语，事实上它是一个神奇的咒语，告诉内核（而不是守护进程）如何处理您的代码。我们说的是脚本的第一行 #!/bin/bash 或可能是 #!/bin/sh，当然。#! 巧妙地充当注释和魔数，告诉内核查找解释器，在本例中为 bash。

Linux 上最常见的 bash shebang 是 #!/bin/bash，但您经常会看到 #!/bin/sh 和 #!/bin/bash -，您可能会看到 #!/usr/bin/env bash、#!/usr/bin/bash 或其他路径。

1 WET: We Enjoy Typing，与 DRY: Don’t Repeat Yourself 相反。





我们建议使用 #!/bin/bash - 或 #!/usr/bin/env bash。我们强烈不建议使用 #!/bin/sh，除非你是有意将代码限制在 Bourne shell 兼容性上，换句话说不使用“bashism”。过去在 Linux 上这样做是可以的，但许多 Linux 发行版已改用 Debian Almquist shell 或 dash 作为 /bin/sh，因为它比 bash 更小更快，运行脚本时速度更快，就像系统启动时使用的所有脚本一样。但当你真正需要 bash 时使用 sh 是一个坏主意，因为正如我们刚才所说，它很脆弱并且掩盖了程序员的意图。

#!/usr/bin/env bash 可移植性更高，因为只要 bash 在 $PATH 中，它就会去找到它。当 bash 不在 /bin/ 中时，这非常有用，这在 BSD、Solaris 和其他非 Linux 系统上就是这种情况。唯一的问题是，生成 env，然后搜索 $PATH，然后用 bash 替换自身的开销更慢。有了现代计算机的资源，这几乎从不重要，但如果你在受限系统上开发或在短时间内运行大量脚本或实例，这种开销会累积起来（示例 9-11）。

### 示例 9-11. Shebang 速度测试

```
$ head examples/ch09/shebang*
==> examples/ch09/shebang-bash.sh <==
#!/bin/bash -
:

==> examples/ch09/shebang-env.sh <==
#!/usr/bin/env bash
:

$ time for ((i=0; i<1000; i++)); do examples/ch09/shebang-bash.sh; done
real    0m3.279s
user    0m1.273s
sys     0m0.915s

$ time for ((i=0; i<1000; i++)); do examples/ch09/shebang-env.sh; done
real    0m4.291s
user    0m1.313s
sys     0m1.425s
```

需要提到的另一个细节是我们展示的尾随 -。这是为了防止非常古老的解释器欺骗攻击；有关详细信息，请参阅 Unix FAQ 的第 4.7 节。Linux 内核（但其他内核不这样做！）将第一个“单词”之后的所有内容（在本例中为 /bin/bash）视为单个参数，因此我们用“结束参数”的连字符“使用”该参数。请注意，这在 #!/usr/bin/env bash 中不需要，因为 bash 部分本身就是参数。
