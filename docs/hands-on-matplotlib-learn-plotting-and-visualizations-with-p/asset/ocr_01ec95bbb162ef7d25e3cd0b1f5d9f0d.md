You can even add a title to the graph as follows:

```
plt.plot(x, y, 'o--')
plt.plot(x, -y, 'o-')
plt.title('y=x and y=-x')
plt.show()
```

The output will have a title as shown in Figure 3-3.

![](bbox=[0.27, 0.28, 0.7, 0.51])

Figure 3-3. Visualizing multiple lines and title

The function linspace(start, stop, number) returns an array of evenly spaced numbers over a specified interval. You must pass it the starting value, the end value, and the number of values as follows:

```
N = 16
x = np.linspace(0, 15, N)
print(x)
```

The previous code creates 11 numbers (0 to 10, both inclusive) as follows:

```
[ 0.  1.  2.  3.  4.  5.  6.  7.  8.  9. 10. 11. 12. 13. 14. 15.]
```

Let’s visualize this as follows:

```
y = x
plt.plot(x, y, 'o--')
plt.axis('off')
plt.show()
```