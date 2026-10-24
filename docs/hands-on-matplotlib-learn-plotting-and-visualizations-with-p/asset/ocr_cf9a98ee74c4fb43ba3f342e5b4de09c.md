The following code produces the same output as shown in Figure 17-5:

```
```python
sns.scatterplot(x='sepal_length', y='sepal_width',
                hue='petal_length', style='species',
                data=iris)
plt.grid('on')
plt.show()
```
```

You can also adjust the sizes of the markers as follows:

```
```python
sns.relplot(x='sepal_length', y='sepal_width',
            size='petal_length', style='species',
            hue='species', data=iris)
plt.grid('on')
plt.show()
```
```

Figure 17-6 shows the output.

![](bbox=[0.219, 0.464, 0.757, 0.761])

**Figure 17-6.** *Scatter plot with colors and custom styles and marker sizes*