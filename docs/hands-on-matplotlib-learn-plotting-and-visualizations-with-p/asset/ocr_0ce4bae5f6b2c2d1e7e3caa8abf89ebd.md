2 0.593635 0.842846 0.755377 A
3 0.210409 0.235510 0.633318 A B
4 0.268419 0.170563 0.478912 A
5 0.526251 0.258278 0.549876 B
6 0.311182 0.212787 0.966183 B A
7 0.100687 0.432545 0.586907 B
8 0.416833 0.879384 0.635664 B A
9 0.249280 0.558648 0.661523 B

You can create box plots in groups of multiple columns (this means the grouping criteria will have multiple columns).

```
plt.figure();
bp = df.boxplot(column=['Col1', 'Col2'], by=['X', 'Y'])
plt.show()
```

Figure 16-17 shows the output.

![](bbox=[0.24, 0.48, 0.78, 0.72])

Figure 16-17. Box plots with groups (multiple columns in the grouping criteria)

Let’s see a bit more complex example with a dataset that has more variation. The following code creates such a dataset:

```
np.random.seed(1234)
df_box = pd.DataFrame(np.random.randn(10, 2), columns=['A', 'B'])
```