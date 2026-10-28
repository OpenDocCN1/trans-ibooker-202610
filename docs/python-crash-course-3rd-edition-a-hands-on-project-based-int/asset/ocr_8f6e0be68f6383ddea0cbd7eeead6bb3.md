that both axes should have equal spacing between tick marks ❸.

[Figure 15-9](#) shows the resulting plot with 5,000 points. The images in this section omit Matplotlib’s viewer, but you’ll continue to see it when you run *rw_visual.py*.

![](bbox=[0.14, 0.24, 0.79, 0.54])

[Figure 15-9](#): *A random walk with 5,000 points*

# ## Generating Multiple Random Walks

Every random walk is different, and it’s fun to explore the various patterns that can be generated. One way to use the preceding code to make multiple walks without having to run the program several times is to wrap it in a `while` loop, like this:

***rw_visual.py***

```
```python
import matplotlib.pyplot as plt

from random_walk import RandomWalk

# Keep making new walks, as long as the program is active.
```
```