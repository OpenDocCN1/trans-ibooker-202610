```
def make_timer(func):
    def wrapper(*args, **kwargs):
        t1 = time.time()
        ret_val = func(*args, **kwargs)
        t2 = time.time()
        print('Time elapsed was', t2 - t1)
        return ret_val
    return wrapper
```

The new function, remember, will be wrapper. It is wrapper (or rather, the function temporarily named wrapper) that will eventually be called in place of func; this wrapper function therefore must be able to take any number of arguments, including any number of keyword arguments. The correct action is to pass along all these arguments to the original function, func. Here's how:

Click here to view code image

```
ret_val = func(*args, **kwargs)
```

Returning a value is also handled here; the wrapper returns the same value as func, as it should. What if func returns no value? That’s not a problem, because Python functions return None by default. So the value None, in that case, is simply passed along. (You don’t have to test for the existence of a return value; there always is one!)
Having defined this decorator, make_timer, we can take any function and produce a wrapped version of it. Then—and this is almost the final trick—we reassign the function name so that it refers to the wrapped version of the function.