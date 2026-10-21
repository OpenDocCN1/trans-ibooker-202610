## 使用 logger

logger 工具（man logger）非常适合将日志直接发送到 syslog，但我们认为它有一个 bug，即 -t 或 --tag 参数不是强制性的，默认只是用户名。我们推荐示例 10-14 中所示的方法。

### 示例 10-14. logger 标签

```
logger --tag "$0[$$]" <msg>
logger --tag "$PROGRAM[$$]" <msg>
logger --tag "${0##*/}[$$]" <msg>
```

① 最简单的情况是使用 $0 作为脚本名称，$$ 作为其 PID。$0 的问题在于它会根据脚本的调用方式而变化，这既嘈杂又不一致。我们更愿意使用 basename。

② 如果你已经像 $PROGRAM 这样使用 basename（示例 8-3），那么也可以在 logger 中使用它。

③ 如果你没有为脚本的 basename 设置类似 $PROGRAM 的变量，你可以在 logger 标签中直接完成。这开始有点晦涩，但它会起作用。尝试 echo "${0##*/}[$$]" 来查看，但从命令行来看它没那么有趣。

如果你要自行记录日志，请参阅“Getting or Using the Date and Time”第 57 页中的 printf 和 time。
