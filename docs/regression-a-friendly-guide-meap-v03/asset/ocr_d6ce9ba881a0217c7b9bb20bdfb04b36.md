```
>>> plt.title("Log(Weight) versus Log(Length) for sharks")
```

Figure 2.17. Scatterplot of log(Weight) versus log(Length) for cartilaginous fish

![](bbox=[0.14, 0.2, 0.76, 0.65])

After all of this work to prepare the sharks dataset, let’s save it for later use:

```
>>> sharks.to_csv("sharks.csv",index=False)
```

This dataset will now be ready to go when we return to it in Chapter 3.
Before moving on to the next example, though, we need to address one aspect of the sharks scatterplot. We can see that the weights for the short cartilaginous fish were missing in the species dataset, since the points in the