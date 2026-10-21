## 轮转旧文件

日志记录很好，归档也很好，但总有一天你需要“轮转”或删除旧文件。有很多惯用的方法可以做到这一点，我们将讨论其中几种并提出一些建议。

许多轮转旧文件的方法都涉及出色的 find 工具，但其中很大一部分还使用了 -exec 和/或 xargs，而在现代版本的 GNU find 中，这些是不必要的。旧的习惯用法是：

```
find /path/to/files -name 'some-pattern' -a -mtime 5 -exec rm -f \{\} \;
```

```
find /path/to/files -name 'some-pattern' -a -mtime 5 | xargs rm -f
```

```
find /path/to/files -name 'some-pattern' -a -mtime 5 -print0 | xargs -0 rm -f
```

① 这极其低效，因为它为每个文件调用一次新的 rm 实例。

② 这更高效，因为它会批量处理文件（xargs），但如果文件名包含空格就会中断并产生意外的行为。

③ 这也可以处理空格，但它仍然比你需要的更复杂。

如果你使用 GNU find 且不担心可移植性，使用示例 10-17 中的代码会简单得多。

### 示例 10-17. 使用 find 删除旧文件

```
find /path/to/files \( -type f -a -name 'some-pattern' -a -mtime 5 \) -delete
```

请记住以下几点：

- 请参阅 man find 并搜索 atime 和 mtime。-mtime +5 表示文件至少在五天前最后修改过，但“五”并不精确；请参阅手册页了解细节讨论。
- 为了测试，使用 -ls 或 -print 而不是 -delete！但完成后不要忘记更改它。
- 请注意，find 会递归进入路径下任何/所有子目录，除非你使用 -maxdepth 来限制它。
- 你想要使用 ( ) 来分组（find 仅匹配所有条件的东西…然后删除它们），但你必须转义它们以防止 shell 自己解释它们。
- 表达式中选项之间假定有 -a “与” 运算符；如果需要，也可以使用 -o 表示“或”。我们喜欢加上 -a，因为显式优于隐式。





+   - `-type f` 表示 *type* 为普通文件，因此它会忽略目录、链接等。
+   - `-name` 很明显但区分大小写。如果需要不区分大小写，可以使用 `-iname`。
