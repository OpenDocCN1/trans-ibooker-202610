## 退出代码

如果没有特别说明，你的 bash 脚本将以最后运行的命令的退出（或返回）代码退出。有时这没问题，有时你可能需要在执行健全性检查失败后或出于其他条件提前退出。

在 bash 中，零表示 *成功*，非零表示失败。这与许多其他语言相反，但原因是有一种成功方式但有许多失败方式，你可能需要表示失败的方式，例如：

```
if grep --quiet "$HOSTNAME" /etc/hosts; then ...
```

bash 退出代码只有八位，因此最大值是 255，但你希望返回代码值保持在 125 及以下，因为 126 及以上已被占用，如表 9-1 所示。

### 表 9-1. bash 退出/返回代码

| Exit/return code | Use/description |
|---|---|
| 0 | Success |
| 1–125 | User-defined failure |
| 2 | bash builtins incorrect usage |
| 126 | Command found but not executable |
| 127 | Command not found |
| 128–255 | Command exited with signal N-128 |

另请参阅 GNU 文档关于退出状态。

用于以“退出代码”退出整个脚本的命令是，等一下，exit n。用于从函数返回的命令是，再次，等一下，return n，其中“n”是之前定义的可选参数。请记住 return 返回的是退出代码，而不是值。“n”可以是任何 shell 表达式，如 5 或 $val 或更复杂的表达式。请记住如果未给出“n”，状态为最后运行命令的状态。

另请参阅我们在第 79 页“HELP!”中关于 exit 0 或 exit 1 的帮助和用法讨论。
