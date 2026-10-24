```
```python
import matplotlib.pyplot as plt

sns.relplot(x='sepal_length',
            y='sepal_width',
            data=iris)
plt.grid('on')
plt.show()
```
```

This produces the scatter plot shown in Figure 17-2.

![](bbox=[0.294, 0.323, 0.68, 0.601])

**Figure 17-2.** *The scatter plot*

You can explicitly specify the type of plot as follows:

```
```python
sns.relplot(x='sepal_length', y='sepal_width',
            data=iris, kind='scatter')
plt.grid('on')
plt.show()
```
```