

- 使用 `readonly` 或 `declare -r` 来定义常量
- 其他变量使用小写
- 函数使用 Mixed_Case
- 用 "_" 代替空格或 CamelCase（记住变量名中不允许使用 "-"）
- 谨慎使用 bash 数组，它们可能难以阅读（参见第 4 章）。`for var in $regular_var` 通常也能工作。
- 尽快用可读的名称替换 $1, $2, .. $N
  - 这样会使一切更易于调试和阅读，同时也便于设置默认值和增添或重新排列参数
- 区分引用类型的不同，例如 `$input_file` 与 `$input_dir`
- 使用一致的 "FIXME" 和 "TODO" 标签，如有需要加上名称和工单号

## Functions

- 一定要使用头部注释
- 好的名称至关重要！
- 函数必须在被使用前定义
  - 将它们分组放在顶部，并在每个函数之间使用两个空行和一个函数分隔符。
  - 不要在函数之间穿插代码！

- 使用 Camel_Case 和 "_" 使函数名与变量名区分开来。
- 使用 `function My_Func_Name {` 而不是 `My_Func_Name() {` 因为它更清晰，也更容易通过 grep -P '^\s*function ' 查找。
- 每个函数都应有注释说明：它做什么，输入（包括 GLOBALS），以及输出
- 当你有有用的、独立的代码片段时，将其做成一个函数。
- 只要你多次使用相同（或基本相同）的代码块，就使用一个函数。如果它们很常见，比如日志记录或发送邮件，考虑创建一个“库”，即一个可以按需 source 的单一公共文件。
  - 为“库”函数加上前缀 "_"，如 `_Log`，或其他前缀。
- 如果在参数中加入“填充”词有助于可读性，可以考虑使用，然后将其定义为 `local junk1="$2" # unused filler`，例如：
  - `_Create_File_If_Needed "/path/to/$file" containing important value`
- 在函数中设置变量时请使用 `local` 内置命令。
  - 但要注意它会掩盖返回码，因此如果使用命令替换，请将声明和赋值分开写，如 `local my_input` 然后 `my_input="$(some-command)"`。
- 对于任何超过约 25 行的函数，用 `} # end of function MyFuncName` 来关闭它，以便在屏幕上更容易追踪

你当前在代码中的位置。对于少于 25 行的函数这是可选的，但如果不会显得杂乱则建议使用。

- 不要使用 `main` 函数，它几乎总是多余的一层。
  - 尽管如此，对于 Python 和 C 程序员来说使用“main”是有意义的，或者如果代码也用作库，并且如果你做大量单元测试时可能需要。
- 考虑在主部分上方使用 2 个空行和一个主分隔符，尤其是在顶部有大量函数和定义时。

## Quoting

- 对变量和字符串加上引号，因为这样会使其更突出并明确你的意图
  - 除非变得过于杂乱
  - 或它们需要不加引号以便展开
- 不要给整数加引号
- 除非需要插值，否则使用单引号
- 不要使用 `${var}` 除非必要，它太杂乱了
  - 但有时确实需要，比如
    `${variable}_suffix` 或
    `${being_lower_cased,,}`
- 对命令替换加上引号，如：`var="$(command)"`

- 对任何测试语句的两侧都始终加上引号，如 [[ "$foo" == bar ]]
  - FIXME-TOOLS: AsciiDoc 又缺少单引号了！
  - 除非你使用 ~=，在这种情况下你不能给正则表达式加引号！

- 考虑在 echo 语句中对变量使用单引号，如 echo "cd to $DIR failed." 因为当变量意外未定义或为空时更清晰
  - FIXME-TOOLS: AsciiDoc 又缺少单引号了！
  - 或 echo "cd to [$DIR] failed." 随你喜欢

- 在 printf 格式中优先使用单引号（参见第 6 章）

## Layout

- 对齐行！在 bash 中多个空格几乎无关紧要（= 两侧除外），而对齐相似的命令使阅读更容易，也便于看到相似性和差异。

- 不要允许尾随空白！这会在 VCS（版本控制系统）中在后续移除时产生噪音。

- 使用 4 个空格缩进，但在 here-doc 中根据需要使用 TAB

- 在约 78 列处换行长行，续行缩进 2 个空格，并在 | 或 > 之前换行，这样在扫描代码时这些部分会突出

- 打开块的代码放在一行中，如：
  - `if expression; then`
  - `for expression; do`
- `case..esac` 中的列表元素缩进 4 个空格，闭合的 `;;` 与该缩进级别相同，每个项目的块也缩进 4 个空格
  - 单行元素应在同一行用 `;;` 闭合
  - 优先对齐每个元素中的 `)`，除非变得杂乱或失控
  - 参见 *Chapter 8* 中的示例代码

## Syntax

- 编写 bash 代码时使用 `#!/bin/bash` 或 `#!/usr/bin/env bash`，不要使用 `#!/bin/sh`
  - FIXME-TOOLS: 三个 # ! 上方存在 Markdown/Asciidoctor 渲染问题
- 除非你 *真的* 确定需要 `$*`，否则使用 `$@`
- 使用 `==` 而不是 `=` 进行相等比较，以减少与赋值的混淆
- 使用 `$()` 而不是 `<code>``</code>` 反引号/反引号
  - FIXME-TOOLS: 反引号存在 Markdown/Asciidoctor 渲染问题
- 使用 `[ [` 而不是 `[`

- 根据需要使用 `0 1` 和 `2 3` 进行整数运算，避免使用 `let` 和 `expr`
- 当简单且可读时使用 `[[ expression ]] && block` 或 `[[ expression ]] || block`。不要使用 `[[ expression ]] && block || block` 因为它不会按你想的那样工作，对于这种情况使用 `if .. (elif .. ) then .. else`

## Other

- 对于“系统”脚本，记录到 syslog，让操作系统处理最终目标、日志轮转等
- 错误信息应发送到 STDOUT，如 `echo 'A Bad Thing happened' >&2`
- 检查外部工具是否可用
- 失败时提供有用的信息
- 设置 `exit` 代码，尤其是在失败时

## Script Template

```
#!/bin/bash -
# Or possibly: #!/usr/bin/env bash
# <Name>: <description>
# Original Author & date:
# Current maintainer?
# Copyright/License?
# Where this code belongs?  (Hosts, paths, etc.)
# Project/repo?
# Caveats/gotchas?
# Usage?  (Better to have `-h` and/or `--help` options!)
# $URL$  # If using SVN
ID=''      # If using SVN
# ____________________________________________________________
```

```
PROGRAM=${0##*/}  # bash version of `basename`

# GLOBAL and constant variables are in UPPER case
LOG_DIR='/path/to/log/dir'

# Functions are in Mixed Case
######################################################################
################
# Define functions

#--------------------------------------------------------------------
----------------
# Example function
# Globals: none
# Input:  nothing
# Output: nothing
function foo {
    local var1="$1"
    ...
} # end of function foo

#--------------------------------------------------------------------
----------------
# Another example function
# Globals: none
# Input:  nothing
# Output: nothing
function bar {
    local var1="$1"
    ...
} # end of function bar

######################################################################
################
# Main
...
```
