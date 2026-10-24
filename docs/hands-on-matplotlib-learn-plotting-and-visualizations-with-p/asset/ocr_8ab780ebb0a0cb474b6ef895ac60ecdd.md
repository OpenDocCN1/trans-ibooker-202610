# CHAPTER 4   REVISITING MATPLOTLIB VISUALIZATIONS

You can also customize the values of the axes as follows:

```
```python
x = np.arange(3)
plt.plot(x, -x**2, x, -x**3, x, -2*x, x, -2**x)
plt.grid(True)
plt.axis([0, 2, -8, 0])
print(plt.axis())
plt.show()
```
```

The statement `plt.axis([0, 2, -8, 0])` sets the values of the axes. The first pair, (0, 2), refers to the limits for the x-axis, and the second pair, (-8, 0), refers to the limits for the y-axis. You can write the previous code with different syntax using the functions `xlim()` and `ylim()` as follows:

```
```python
x = np.arange(3)
plt.plot(x, -x**2, x, -x**3, x, -2*x, x, -2**x)
plt.grid(True)
plt.xlim([0, 2])
plt.ylim([-8, 0])
print(plt.axis())
plt.show()
```
```

Both the examples produce the same output, as shown in Figure 4-9.

![](bbox=[0.263, 0.594, 0.727, 0.829])

***Figure 4-9.  Customizing the axes***