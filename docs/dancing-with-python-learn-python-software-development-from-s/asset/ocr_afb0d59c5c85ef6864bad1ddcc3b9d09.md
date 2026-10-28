```
circuit.ccx(0, 1, 2)
circuit.h(2)
for q in range(3): circuit.x(q)
```

```
circuit.draw(**draw_kwargs)
```

![](bbox=[0.35, 0.21, 0.66, 0.32])

# Exercise 11.13

Verify that the last two circuits are oracles for |111⟩ and |000⟩. What is the oracle for |010⟩? |101⟩?

# 11.4 Inversion about the mean

Suppose I have four data points and their mean:

```
data = [3, 1, 2, 5]
mean = float(sum(data)) / len(data)
mean
```

2.75

I show these values in the bar chart on the left-hand side of Figure 11.6.

![](bbox=[0.18, 0.67, 0.8, 0.85])

Figure 11.6: Inversion about the mean for four data points