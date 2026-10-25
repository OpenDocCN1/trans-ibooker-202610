```
import array
a = array.array('h', [1, 2, 3])
```

```
import array
a = array.array('h', range(1000))

import array
a = array.array('l', range(1_000_000))

a_list = list(range(1, 1_000_001))
print(sum(a_list))
```