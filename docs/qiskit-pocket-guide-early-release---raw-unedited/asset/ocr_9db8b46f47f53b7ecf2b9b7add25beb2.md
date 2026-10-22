# UGate

The UGate class creates a single-qubit U gate with 3 Euler angles.

Signature:

Appearance:

Matrix:

```
UGate(theta, phi, lam, label=None)
```

![](bbox=[0.52, 0.315, 0.7, 0.39])

⎛ cos(θ)  -e^(iλ) sin(θ) ⎞
⎝ e^(iφ) sin(θ)  e^(i(φ+λ)) cos(θ) ⎠

# XGate

The XGate class creates a single-qubit X gate that performs a π rotation around the X axis.

Signature:

Appearance:

Matrix:

```
XGate(label=None)
```

![](bbox=[0.5, 0.565, 0.68, 0.64])

⎛ 0 1 ⎞
⎝ 1 0 ⎠

# YGate

The YGate class creates a single-qubit Y gate that performs a π rotation around the Y axis.

Signature:

Appearance:

Matrix:

```
YGate(label=None)
```

⎛ 0  -i ⎞
⎝ i   0 ⎠