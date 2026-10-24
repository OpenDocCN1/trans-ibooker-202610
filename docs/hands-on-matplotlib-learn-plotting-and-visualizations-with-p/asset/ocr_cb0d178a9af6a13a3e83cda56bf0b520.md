Figure 4-6 shows the output.

![](bbox=[0.25, 0.17, 0.7, 0.38])

Figure 4-6. Multiline graph, random data

In this example, we generated the data in a random way using the routine np.random.randn(). Since this routine will generate the random data, the output will be different every time we execute it. So, the output you will see will be different every time you execute the code.

# Grid, Axes, and Labels

Now you will learn how to enable a grid in the visualizations. This can be done with the statement plt.grid(True). You will also learn how to manipulate the limits of axes. But before that, you will quickly learn how to save a visualization as an image on the hard disk. Look at the following code:

```
x = np.arange(3)
plt.plot(x, -x**2, x, -x**3, x, -2*x, x, -2**x)
plt.grid(True)
plt.savefig('test.png')
plt.show()
```

The statement plt.savefig('test.png') saves the image in the current directory of the Jupyter Notebook file. Figure 4-7 shows the output.