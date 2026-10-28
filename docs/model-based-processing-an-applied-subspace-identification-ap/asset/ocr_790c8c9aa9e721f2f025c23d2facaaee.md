# 1.6

The elevation of a tracking telescope is controlled by a DC motor. It has a moment of inertia J and damping B due to friction, and the equation of motion is given by

```
J d²θ/dt² + B dθ/dt = Tₘ + T_d
```

where Tₘ and T_d are the motor and disturbance torques and θ is the elevation angle. Assume a sensor transforms the telescope elevation into a proportional voltage that is contaminated with noise. Develop the signal and noise models for the telescope and identify all of the model-based processor components.

# 1.7

Suppose we have a two-measurement system given by

```
y = [3; 4] + v
```

where R_vv = diag[1, 0.1].

+   (a) What is the batch least-squares estimate (W = I) of the parameter x, if y = [7 21]?'

+   (b) What is the batch weighted least-squares estimate of the parameter x with W selected for minimum variance estimation?

# 1.8

Calculate the batch and sequential least-squares estimate of the parameter vector x based on two measurements y(1) and y(2) where

```
y(1) = C(1)x + v(1) = [2; 1]
```

```
y(2) = c' x + v(2) = 4
```

```
C = [1 1; 0 1],   c'(1) = [1 2],   W = I
```