The common operations on an array or list are:

+   - Declare a variable as a list
+   - Assign one or more values to it
+   - If treating it as a stack (think plates in a cafeteria; FIFO: first on, first off, or first in, first out)
+     - push
+     - pop
+   - Display (dump) all values for debugging or reuse
+   - Reference one or all values (`for`, or `for each`)
+   - Reference a sub-set (slice) of values
+   - Delete one or more values
+   - Delete the entire list

Rather than talk about all of that, we’re going to show you instead, and you can just pick out the idioms you need when you need them:

# Example 4-1. Bash List Example: Code

```
#!/usr/bin/env bash
# lists.sh: bash list example code
# Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch07/lists.sh
#
____________________________________________________________
________________________________

# Declare a list
①
# declare -a mylist    # Can do this, or `local -a` or
`readonly -a` or:
mylist[0]='foo'        # This both declares and assigns to
mylist[0]

# Push or assign, note the +=
②
```