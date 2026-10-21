## bash Linter

使用 linter 代替风格指南或作为补充也可能很方便。说实话，我们不用，但我们认为应该涵盖一下。我们知道的是 shellcheck，我们对结果有复杂的感受；它对引号特别挑剔，我们并不总是同意它关于引号或一般方面的建议，但这些东西或多或少是可以调整的。话虽如此，它仍然非常酷，值得一看。

- bash linter: https://www.shellcheck.net
  - https://github.com/koalaman/shellcheck (Haskell)
  - History: https://www.vidarholen.net/contents/blog/?p=859
  - Checks: https://github.com/koalaman/shellcheck/wiki/Checks
  - Tweaking: https://github.com/koalaman/shellcheck/wiki/Directive
  - Ignoring errors: https://github.com/koalaman/shellcheck/wiki/Ignore
  - Note, Eol (2020) CentOS-6 is too old: https://github.com/koalaman/shellcheck/wiki/CentOS6

如果你通过包系统安装，请记住版本可能相当旧。Linux 版本是一个包含单个编译二进制文件的 tarball，所以很容易将当前版本放到你的路径中的某个位置。
