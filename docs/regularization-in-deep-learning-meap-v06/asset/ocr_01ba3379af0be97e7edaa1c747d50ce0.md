that balances the bias and the variance is also reflected as the “sweet spot” in figure 1.7 back in chapter 1.

To see the increased variance as the model becomes complex, we will employ additional feature engineering by increasing the number of polynomial basis functions to a higher degree. Instead of running the same codes multiple times, each with a different polynomial degree, it is better to wrap the polynomial transformation into a more general function. Designing a reusable function makes it flexible in specifying the degree of polynomial to be performed in the transformation. The following function achieves this by first creating a matrix as a placeholder based on the pre-defined degree of the polynomial, followed by filling in the matrix using polynomial functions.

# Listing 2.12 Generate the new design matrix based on polynomial terms

```
def polynomial(x, num_basis=2):
    Phi = np.zeros((x.shape[0], num_basis)) #A
    for i in range(num_basis): #B
        Phi[:, i:i+1] = x**i
    return Phi
#A Create the matrix of zeros as placeholder
#B Fill in each column based on the sequence of polynomials
```

We can create a new design matrix using this utility function by passing in the original design matrix and specifying the number of basis functions. Figure 2.24 shows the fitted model using three different numbers of basis functions along with the respective cost. The model becomes increasingly unstable as the number of basis functions grows. Refer to the accompanying notebook at https://github.com/jackliu333/regularization_in_deep_learning for the full training codes.

![](bbox=[0.178, 0.58, 0.892, 0.82])

Figure 2.24 Fitting three models using different numbers of polynomial basis functions. The left plot shows a better fit compared with the model using up to the 2nd degree polynomial. When the degree of polynomial grows to 10 or even 15, the model perfectly fits all the data points. However, this comes at the cost of high variance, especially in the right regions in the middle and right plot.