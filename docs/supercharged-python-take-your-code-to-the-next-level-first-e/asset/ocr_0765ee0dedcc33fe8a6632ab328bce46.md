```
# File run_me.py ------------------------------------
from mod_a import *          # This will fail.

print(_a)
print(__b)

# File mod_a.py ------------------------------------
_a, __b = 10, 100

# File run_me.py ------------------------------------
from mod_a import _a, __b   # This now works.

print(_a)
print(__b)

# File a_mod.py ------------------------------------
_a, __b = 10, 100
```