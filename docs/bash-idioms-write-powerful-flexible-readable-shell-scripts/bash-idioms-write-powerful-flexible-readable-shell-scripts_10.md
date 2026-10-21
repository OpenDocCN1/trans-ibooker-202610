## Using a Library

如果您有大量经常使用的代码，也许是一个使用 printf %(datefmt)T 的 Log 函数（参见第 57 页的“Getting or Using the Date and Time”），将其放入库中以减少“WET”¹ 代码和维护是有意义的。当然，一旦这样做，您就会遇到通常的库问题：部署和调用它。部署取决于您自己的通用流程和实践，但我们可以帮助调用它。

具有讽刺意味的是，您将有重复的“样板”代码来调用您的库，并且您无法将其提取到库中，否则导入过程会变得循环。但由于您有用于头注释等的模板（对吧？），您可以只添加示例 9-10 中显示的代码。

# Example 9-10. Sourcing a bash library

```
# Source our library
source /path/to/global/bash-library.lib || {
    echo "FATAL: $0 can't source '/path/to/global/bash-library.lib'"
    exit 1
}
```

我们本可以定义 GLOBAL_LIBRARY=/path/to/global/bash-library.lib，但对于样板代码中的两次使用，似乎不值得。如果您足够聪明，您会将一组全局配置变量构建到该库中，并且您将允许使用节点本地 drop-in 目录（参见第 93 页的“Drop-in Directories”）覆盖变量甚至库函数。
