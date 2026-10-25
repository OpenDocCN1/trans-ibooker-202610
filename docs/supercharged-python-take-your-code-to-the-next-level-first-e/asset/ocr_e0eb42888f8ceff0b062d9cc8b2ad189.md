```
def pr_named_vals(**kwargs):
    for k in kwargs:
        print(k, ':', kwargs[k])
>>> pr_named_vals(a=10, b=20, c=30)
a : 10
b : 20
c : 30
```

```
def pr_vals_2(*args, **kwargs):
    for i in args:
        print(i)
    for k in kwargs:
        print(k, ':', kwargs[k])
pr_vals_2(1, 2, 3, -4, a=100, b=200)
```

```
import time
def make_timer(func):
    def wrapper():
        t1 = time.time()
        ret_val = func()
        t2 = time.time()
        print('Time elapsed was', t2 - t1)
        return ret_val
    return wrapper
```