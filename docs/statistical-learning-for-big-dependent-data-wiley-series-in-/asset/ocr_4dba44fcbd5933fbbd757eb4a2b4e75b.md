![](bbox=[0.22, 0.1, 0.78, 0.34])

approximations with some activation function and network structure. Indeed, there have been many approaches to obtain universal approximation functions. For instance, we can approximate any continuous function in an interval by using polynomials, by Taylor theorem, or sinusoidal functions, as in Fourier analysis. Kolmogorov (1957) showed that all continuous functions of m variables can be well approximated in terms of finite superpositions and compositions of a small number of functions of one variable. Instead of functions of each variable, NNs search for approximations of functions of linear combinations of all input variables. Cybenko (1989) and Hornik et al. (1989) proved that any continuous function of m real variables can be well approximated by compositions of finite linear combinations of the variables, which implies that a feedforward NN with a single hidden layer of arbitrary width, that is, with a large number of neurons or nodes, can approximate, with any given precision, any continuous function.

The function used to process information from one layer to the next of a network is referred to an activation function. Different layers may use different functions even though the same activation function is commonly used except for the output layer. The property of universal approximation for a single layer NN is valid for any activation function that it is not polynomial. The activation function for the output layer is determined by whether the dependent variable is continuous or discrete. Details are given later. Many activation functions have been proposed in the literature, but the following ones are commonly used in the traditional NN. Section 8.3.3 provides some others that are used in the deep learning or machine learning.

+   1. Logistic function (or Sigmoid function):

```
$$g(x) = \frac{1}{1 + \exp(-x)} = \frac{\exp(b + \sum_{i=1}^m w_i x_i)}{1 + \exp(b + \sum_{i=1}^m w_i x_i)},$$
```

where \( x = (x_1, \dots, x_m)' \) with m being the number of nodes in the previous layer, \( b \) is called the *bias*, and \( w_i \) are the weights. This function is the one used in logistic regression, and it transforms a continuous variable in the real line into a variable between zero and one.