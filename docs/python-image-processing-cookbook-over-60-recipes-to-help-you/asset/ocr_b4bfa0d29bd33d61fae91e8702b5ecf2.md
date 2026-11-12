# Getting ready

As usual, let's start by importing the required libraries:

```
from skimage.exposure import cumulative_distribution
from skimage.color import rgb2gray
import matplotlib.pyplot as plt
import numpy as np
```

# How to do it...

Perform the following steps to implement histogram matching:

+   1. Let's implement the histogram-matching algorithm with
+   the hist_matching() function, which accepts the original and the template
+   image's cdf along with the original image:

```
def hist_matching(c, c_t, im):
    b = np.interp(c, c_t, np.arange(256))
    # find closest matches to b_t
    pix_repl = {i:b[i] for i in range(256)}
    # dictionary to replace the pixels
    mp = np.arange(0,256)
    for (k, v) in pix_repl.items():
        mp[k] = v
    s = im.shape
    im = np.reshape(mp[im.ravel()], im.shape)
    im = np.reshape(im, s)
    return im
```

+   2. Compute cdf of an image with the following function:

```
def cdf(im):
    c, b = cumulative_distribution(im)
    for i in range(b[0]):
        c = np.insert(c, 0, 0)
    for i in range(b[-1]+1, 256):
        c = np.append(c, 1)
    return c
```