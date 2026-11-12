```
#A Create an array of inputs
#B Randomly initialize the weights
#C Compute hidden layer activation and final output
#D Plot the activation output of the five hidden neurons
```

Running the code listing above generates figure 4.4, which suggests that the ReLU activations from the five neurons in the hidden layer are of similar shape. However, they differ in three aspects: the turning point (threshold), the slope of the linear segment, and the direction of the activation (positive or negative slope). These activations are derived and learned features automatically extracted in the hidden space compared to the manually engineered polynomial features in the previous chapter. As optimization proceeds, these features will jointly form a better representation of the original input data, each presenting a different perspective at various levels of granularity. They can also be interpreted as a set of derived basis functions.

![](bbox=[0.25, 0.37, 0.9, 0.78])

Figure 4.4 Visualizing the activation output of the five neurons in the hidden layer. Each activation is a ReLU function with similar shape, differing in the location of the turning point, the magnitude and the direction of the slope.

The model output from the last layer is then calculated via a linear combination of these basis functions. To further probe into the outcome of the series of functional transformations,