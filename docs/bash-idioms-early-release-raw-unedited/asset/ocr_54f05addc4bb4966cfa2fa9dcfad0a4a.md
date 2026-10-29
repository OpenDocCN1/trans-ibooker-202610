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