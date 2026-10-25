```
def sieve(n):
    b_list = [True] * (n + 1)
    for i in range(2, n+1):
        if b_list[i]:
            for j in range(i*i, n+1, i):
                b_list[j] = False
    primes = [i for i in range(2, n+1) if b_list[i]]
    return primes

import numpy as np

def np_sieve(n):
    # Create B, setting all elements to True.
    B = np.ones(n + 1, dtype=np.bool)
    B[0:2] = False
    for i in range(2, n + 1):
        if B[i]:
            B[i*i: n+1: i] = False
    return np.arange(n + 1)[B]
```