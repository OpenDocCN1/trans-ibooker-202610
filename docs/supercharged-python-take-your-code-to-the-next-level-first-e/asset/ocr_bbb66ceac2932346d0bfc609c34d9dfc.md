```
0.2889516828729947
>>> np.std(A)
0.2889516828729947
```

The results, as you can see, are precisely the same in both cases—calculating standard deviation “the hard way” and getting it from `np.std`—which is good evidence that the **numpy** routines are following the same algorithm.

It’s instructive to run the standard deviation function on an even larger array—say, an array of 1 million random numbers— and the equivalent code in Python, using standard lists.

Now comes the interesting part: If you benchmark this technique with a list of 1 million elements in size, against a **numpy** version with an array containing the same data, the **numpy** version—getting the standard deviation directly—beats out the non-**numpy** version by a factor of more than 100 to 1!

Here’s the full benchmark code:

```
import numpy as np
import time
import numpy.random as ran

def get_std1(ls):
    t1 = time.time()
    m = sum(ls)/len(ls)
    ls2 = [(i - m) ** 2 for i in ls]
    sd = (sum(ls2)/len(ls2)) ** .5
    t2 = time.time()
    print('Python took', t2-t1)

def get_std2(A):
    t1 = time.time()
    A2 = (A - np.mean(A)) ** 2
```