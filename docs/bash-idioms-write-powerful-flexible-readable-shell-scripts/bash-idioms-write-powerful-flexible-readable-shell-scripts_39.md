## 脚本模板

这是一个示例骨架或模板脚本，你可以复制它作为提醒，并在创建新脚本时节省一些（好吧，也许相当多的）输入：

```
#!/bin/bash -
# 或可能是： #!/usr/bin/env bash
# <Name>: <description>
# Original Author & date:
# Current maintainer?
# Copyright/License?
# Where this code belongs? (Hosts, paths, etc.)
# Project/repo?
# Caveats/gotchas?
# Usage? (Better to have `-h` and/or `--help` options!)
# $URL$  # If using SVN
ID=''  # If using SVN
#
PROGRAM=${0##*/}  # bash version of `basename`

# Unofficial bash Strict Mode?
#set -euo pipefail
### CAREFUL: IFS=$'\n\t'

# GLOBAL and constant variables are in UPPER case
LOG_DIR='/path/to/log/dir'

### Consider adding argument handling to YOUR template; see:
# examples/ch08/parseit.sh
# examples/ch08/parselong.sh
# examples/ch08/parselonghelp.sh

# Functions are in Mixed Case
######################################################################
```

```
# Define functions

#-------------------------------------------------------------------------------
# Example function
# Globals: none
# Input:  nothing
# Output: nothing
function Foo {
    local var1="$1"
    ...
} # End of function foo

#-------------------------------------------------------------------------------
# Another example function
# Globals: none
# Input:  nothing
# Output: nothing
function Bar {
    local var1="$1"
    ...
} # End of function bar

###############################################################################
# Main

# Code...
```
