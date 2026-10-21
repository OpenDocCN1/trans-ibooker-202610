## 语法

以下是与语法相关的风格指南：

- 编写 bash 代码时使用 #!/bin/bash - 或 #!/usr/bin/env bash，而不是 #!/bin/sh。
- 除非你非常确定需要 $*，否则使用 $@。
- 使用 == 而不是 = 进行相等比较，以减少与赋值的混淆。
- 使用 $( ) 而不是 `` 反引号/反引号。
- 使用 [[ 而不是 [（除非你需要 [ 用于可移植性，例如 dash）。
- 根据需要使用 (( )) 和 $(( )) 进行整数运算；避免使用 let 和 expr。
- 当简单且可读时，使用 [[ expression ]] && block 或 [[ expression ]] || block。不要使用 [[ expression ]] && block || block，因为它不做你认为它做的事情；对于这种情况，使用 if .. then .. (elif ..) else .. fi。
- 考虑使用“非官方 bash 严格模式”（参见第 96 页的“Unofficial bash Strict Mode”）。
  - set -euo pipefail 将防止或暴露许多简单错误。
  - 注意这一点，并谨慎使用它（如果你使用它）：IFS=$'\n\t'。




