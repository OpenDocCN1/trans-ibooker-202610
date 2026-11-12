```
# Calculate derivative of Z2 with respect to B2
dZ2_dB2 = diff(Z2, B2)
print("dZ2_dB2 = ", dZ2_dB2) # 1
```

```
# Calculate derivative of A1 with respect to Z1
relu = lambda x: Max(x, 0)
_A1 = relu(Z1)

d_relu = lambda x: x > 0 # Slope is 1 if positive, 0 if negative
dA1_dZ1 = d_relu(Z1)
print("dA1_dZ1 = ", dA1_dZ1) # Z1 > 0
```

```
# Calculate derivative of Z1 with respect to W1
_Z1 = X*W1 + B1
dZ1_dW1 = diff(Z1, W1)
print("dZ1_dW1 = ", dZ1_dW1) # X
```

```
# Calculate derivative of Z1 with respect to B1
dZ1_dB1 = diff(Z1, B1)
print("dZ1_dB1 = ", dZ1_dB1) # 1
```

Notice that ReLU was calculated manually rather than using SymPy's + diff() + function. This is because derivatives work with smooth curves, not jagged corners which exists on ReLU. But it's easy to hack around that simply by declaring the slope to be 1 for positive numbers, and 0 for negative numbers. This makes sense because negative numbers have a flat line with slope 0. But positive numbers are left as-is with a 1-to-1 slope.

These partial derivatives can be chained together to create new partial derivatives with respect to the weights and biases. Let's get all four partial derivatives for the weights in $W_1$, $W_2$, $B_1$, and $B_2$ with respect to the cost function. We already walked through $\frac{dC}{dw_2}$. Let's show it alongside the other three chained derivatives we need.

```
\frac{dC}{dW_2} = \frac{dZ_2}{dW_2} \frac{dA_2}{dZ_2} \frac{dC}{dA_2} = (A_1) \left( \frac{e^{-Z_2}}{(1 + e^{-Z_2})^2} \right) (2A_2 - 2y)
```

```
\frac{dC}{dB_2} = \frac{dZ_2}{dB_2} \frac{dA_2}{dZ_2} \frac{dC}{dA_2} = (1) \left( \frac{e^{-Z_2}}{(1 + e^{-Z_2})^2} \right) (2A_2 - 2y)
```

```
\frac{dC}{dW_1} = \frac{dC}{DA_2} \frac{DA_2}{dZ_2} \frac{dZ_2}{dA_1} \frac{dA_1}{dZ_1} \frac{dZ_1}{dW_1} = (2A_2 - 2y) \left( \frac{e^{-Z_2}}{(1 + e^{-Z_2})^2} \right) (W_2) (Z_1 > 0) (X)
```

```
\frac{dC}{dB_1} = \frac{dC}{DA_2} \frac{DA_2}{dZ_2} \frac{dZ_2}{dA_1} \frac{dA_1}{dZ_1} \frac{dZ_1}{dB_1} = (2A_2 - 2y) \left( \frac{e^{-Z_2}}{(1 + e^{-Z_2})^2} \right) (W_2) (Z_1 > 0) (1)
```