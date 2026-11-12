![](bbox=[0.283, 0.133, 0.679, 0.376])

Figure 2.14 Contour plot of the cost as a function of two parameters. The points on the same dashed line show equal cost. The objective is to reach the middle point where the cost is the lowest, starting from the point at the star. The arrow shows the direction of travel in the next iteration.

The optimization process can gradually reduce the cost to a local minimum by iteratively applying the gradient descent update to the parameters. The code listing below performs the updates for a fixed number of iterations, with the temporary location of the parameters plotted at each 100th iteration.

# Listing 2.8 Performing gradient descent update

```
max_iters = 1000 #A
learning_rate = 0.001 #B
f, ax = plt.subplots() #C
cp = ax.contour(w0_grid, w1_grid, Q_grid, colors='black', linestyles='dashed',
levels=[0, 0.5, 1, 2, 4, 8, 16, 32, 64, 128, 256, 512]) #C
ax.plot(w[0], w[1], 'g*', markersize=10) #C
plt.clabel(cp, inline=True,
fontsize=10) #C
for i in range(max_iters): #C
w0_grad = -2*(y - w[1][0] - w[0]).sum() #C #D
w1_grad = -2*((X - w[1][1]*X - w[0])).sum() #C
w[0] = w[0] - learning_rate*w0_grad #C #E
w[1] = w[1] - learning_rate*w1_grad #C #E
if i % 100 == 0: #C #F
ax.plot(w[0], w[1], 'g*', markersize=10) #C #F
plt.xlabel('w0') #C
plt.ylabel('w1') #C
plt.show() #C
```

#A Maximum iterations for gradient descent update
#B Learning rate or step size per update
#C Plot the cost function as a contour plot
#D Compute the gradients
#E Update the parameters
#F Plot the current parameters at every 100th iteration