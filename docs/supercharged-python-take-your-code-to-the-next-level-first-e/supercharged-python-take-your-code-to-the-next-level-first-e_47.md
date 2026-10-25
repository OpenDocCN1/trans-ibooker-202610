## 循环与作用域示例

```python
# Print members of the Beatles on separate lines
beat_list = ['John', 'Paul', 'George', 'Ringo']
for guy in beat_list:
    print(guy)

# Define function to calculate 1 * 2 * ... n
def factorial(n):
    prod = 1
    for n in range(1, n + 1):
        prod *= n
    return(prod)

account = 1000

def clear_account():
    account = 0   # Oops, create new var, as a local

clear_account()
print(account)   # Prints 1000, this is an error!

account = 1000

def clear_account():
    global account    # Don't make account a local var.
    account = 0       # Reset global copy to 0!

clear_account()
print(account)        # Prints 0, not 1000.

age = int(input('Enter your age: '))
if 12 < age < 20:
    print('You are a teenager.')

age = int(input('Enter age: '))
if age < 13:
    print('Hello, spring chicken!')
elif age < 20:
    print('You are a teenager.')
    print('Do not trust x, if x > 30.')
else:
    print('My, my. ')
    print('We are not getting any younger are we?')

import module
import module as short_name
from module import sym1 [, sym2]...
from module import *
```
