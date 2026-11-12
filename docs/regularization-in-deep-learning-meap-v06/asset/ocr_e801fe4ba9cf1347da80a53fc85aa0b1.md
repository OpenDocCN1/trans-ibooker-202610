```
**Listing 2.10 Optimization using SGD**

```
w = w_original #A
max_iters = 6000 #B
learning_rate = 0.001 #C
f, ax = plt.subplots() #D
cp = ax.contour(w0_grid, w1_grid, Q_grid, colors='black', linestyles='dashed',
                levels=[0, 0.5, 1, 2, 4, 8, 16, 32, 64, 128, 256, 512])
ax.plot(w[0], w[1], 'g*', markersize=5)
plt.clabel(cp, inline=True,
           fontsize=10)
for i in range(max_iters):
    index = np.random.randint(X.shape[0]-1)
    w0_grad = -2*(y[index]- w[1]*X[index] - w[0])
           w1_grad = -2*(X[index]*(y[index] - w[1]*X[index] - w[0]))
           w[0] = w[0] - learning_rate*w0_grad
           w[1] = w[1] - learning_rate*w1_grad
           if i % 100 == 0: #E
               ax.plot(w[0], w[1], 'g*', markersize=5)
plt.xlabel('w0')
plt.ylabel('w1')
plt.show()
```

#A Reset to original weights
#B maximum iterations for gradient descent update
#C learning rate or step size per update
#D plot the cost function as a contour plot
#E plot the current parameters at every 100th iteration
```

Figure 2.17 shows the travelling path of the weights using SGD, which looks wigglier compared with the path using the full batch gradient descent. Smoothing the volatile ups and downs and therefore accelerating the updates using optimizer variants such as momentum and adam, however, may not necessarily contribute to its generalization property. It turns out that such volatility is an important reason behind the good generalization performance of models trained using SGD. We will cover more on this point in a later section.