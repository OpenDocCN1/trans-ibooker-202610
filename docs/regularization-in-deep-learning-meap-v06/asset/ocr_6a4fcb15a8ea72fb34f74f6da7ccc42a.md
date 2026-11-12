we rely on the coding listing below to visualize the output of the fitted model along with the intersections between different ReLU activations in the hidden layer. We also plot the breakpoints of two linear segments as an indicator of model flexibility, where the breakpoint is identified by setting the output of ReLU operation to zero and solving for the value of the input.

```
Listing 4.2 Visualizing the final output of the two-layer neural network
plt.figure(figsize=(8,6))
plt.plot(xs, final_output[0,:], 'k:')
breakpoints = (-b1/W1).T #A
f_breakpoints = np.dot(W2,relu(W1**breakpoints + b1)) #B
plt.plot(breakpoints, f_breakpoints, '.', markersize=18);
plt.xlabel('$x$');
plt.ylabel('$f(x)$');
```

#A Breakpoint is the input location where ReLU output is zero
#B Calculate model output at the breakpoints

Figure 4.5 contains the output of the codes above. The final output is simply a linear combination of the hidden layer’s outputs \(\phi(w_{1x} + b_1)\) weighted by \(w_2\). By comparing with figure 4.4, we see that these turning points (stored in the `breakpoints` variable) are precisely the crossover points among the multiple ReLU activations of the hidden layer. In between these breakpoints are linear segments where the function behaves linearly within the region. The final fitted model is thus piecewise linear with turning points defined by the activation functions.