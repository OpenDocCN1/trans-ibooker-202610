and calculating the predicted outputs based on the initial set of randomly generated weights.
The code snippet below shows how to visualize the prediction in Python.

```
X_vals = np.linspace(X.min(), X.max(), 100)[:, np.newaxis] #A
X_vals_new = np.hstack((np.ones_like(X_vals), X_vals))
y_vals = X_vals_new@w #B

fig, ax = plt.subplots() #C
ax.scatter(X, y, c='black') #C
ax.set_xlabel('x', fontsize=25) #C
ax.set_ylabel('y', fontsize=25) #C
ax.plot(X_vals, y_vals, linewidth=2) #C
plt.show()#C
```

#A Create a list of placeholders on the x axis and explicitly specify the column dimension
#B Generate the list of predictions
#C Plot the model

![](bbox=[0.22, 0.37, 0.6, 0.59])

Figure 2.7 The initial model as represented by the straight line. The fitting is based on the set of weights
before optimization is performed and is thus off-target.

As shown in figure 2.7, the initial fitting is off target and needs to be optimized. The
optimization is performed by tuning the two parameters in a direction that minimizes a pre-
defined cost function. We now introduce the cost component, followed by a detailed
discussion on the optimization procedure.

# 2.3 The cost function

Each model has an associated cost function to measure the goodness of fit to the training
data. Given a prediction f(x_i;w) for the input x_i and the corresponding target y_i for the i^{th} data
pair, the scalar difference or residual r_i in a regression setting can be calculated by taking
their difference, i.e. r_i = y_i - f(x_i;w). These residuals, when reflected on a graph, refer to the
vertical distances between the prediction and the actual training data points on the graph.
There are multiple ways to aggregate these individual residuals, with the sum of squared