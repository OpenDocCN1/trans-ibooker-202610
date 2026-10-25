```
from math import log2, ceil

n = int(input('Enter size of range: '))
x = ceil(log2(n + 1))
print('Max. number of guesses needed is', x)

Enter size of range: 50
Max. number of guesses needed is 6.
```