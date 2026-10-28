Think of the “e” in e₀ as the “error” between the data and the ideal line L. You may see some people use the Greek letter epsilon = ε instead of e.

We call the following technique the linear least squares algorithm because we want to minimize

e₁² + e₂² + e₃²

More generally, this sum divided by the number of observations is called the Mean Squared Error or MSE:

MSE = (e₁² + e₂² + e₃²) / 3

This is an example of a cost function. We define and use these frequently in machine learning and, more generally, in optimization problems. Minimizing a cost function’s output is often the goal in best fitting a machine learning model to training data.

Another cost function is Mean Absolute Error, or MAE: where we take the absolute value of the eⱼ instead of squaring them.

MAE = (|e₁| + |e₂| + |e₃|) / 3

We considered absolute value versus squaring in section 14.1.3 when we defined absolute deviation and standard deviation.

We have three points

(x₁, y₁) = (2.0, 2.0)
(x₂, y₂) = (4.0, 5.0)
(x₃, y₃) = (8.0, 5.0)

Let y = m x + b be the equation of the line L. We wish to find m and b so that

(y₁ − m x₁ − b)² + (y₂ − m x₂ − b)² + (y₃ − m x₃ − b)²

is as small as possible.

The math to compute this is beyond the scope of this book, but if we let μₓ be the mean of the x coordinates and μᵧ be the mean of the y coordinates of our original points, then

m = [(x₁ − μₓ)(y₁ − μᵧ) + (x₂ − μₓ)(y₂ − μᵧ) + (x₃ − μₓ)(y₃ − μᵧ)] / [(x₁ − μₓ)² + (x₂ − μₓ)² + (x₃ − μₓ)²]
b = μᵧ − m μₓ