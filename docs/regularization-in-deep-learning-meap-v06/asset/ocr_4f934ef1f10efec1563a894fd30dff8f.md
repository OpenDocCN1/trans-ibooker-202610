In the context of SGD, the gradient will be calculated based on the cost of the \(i^{th}\) single example, namely,

```
\( Q_{i(t)}(w) = Q_i(w) = \frac{1}{2}(y_i - x_i w)^2 \)
```

Again, setting the gradient to zero gives the optimal solution \(w^*\) for the \(i^{th}\) single example:

```
\(\nabla_w Q_l(w) = (y_i - x_i w)x_i = 0\nw_i^* = \frac{x_i y_i}{x_i^2} = \frac{y_i}{x_i}\)
```

Where we have a total of \(N\) optimal solutions \(\{w^*\}_{i=1}^N\) based on \(N\) randomly selected single examples. Out of this sequence of optimal solutions, we can obtain its highest and lowest candidates, i.e., \(\max_i\{w_i^*\}\) and \(\min_i\{w_i^*\}\), respectively. An interesting question pops up: how does the full-batch solution \(w^*\) compare with \(\min_i\{w_i^*\}\) and \(\max_i\{w_i^*\}\)?

To facilitate the analysis, let us assume that all input observations are positive, i.e., \(x_i > 0\) for all \(i \in \{1,...,N\}\). This is not an overwhelming assumption since we could shift all observations by the same constant until they are positive. Let us further denote \(U = \max_i\{w_i^*\}\) and \(L = \min_i\{w_i^*\}\). We can proceed with the following observation:

```
\(\frac{y_i}{x_i} \le U = \frac{y_U}{x_U}\)
```

Where we use \((x_U, y_U)\) to denote the example that gives the maximum solution \(\max_i\{w_i^*\}\). Reshaping the terms and multiplying both sides by \(x_i\), we have:

```
\(x_U y_i \le x_i y_U\nx_U x_i \le x_i^2 y_U\)
```

Now, adding over all \(i \in \{1,...,N\}\) and switching terms give:

```
\(\sum_{i=1}^N x_U x_i y_i \le \sum_{i=1}^N x_i^2 y_U\nx_U \sum_{i=1}^N x_i y_i \le y_U \sum_{i=1}^N x_i^2\nw^* = \frac{\sum_{i=1}^N x_i y_i}{\sum_{i=1}^N x_i^2} \le \frac{y_U}{x_U} = U\)
```

What this means is that the optimal solution using the full batch is upper bounded by the maximum solution based on a single observation.

We can further prove the lower bound in a similar fashion as follows:

```
\(\frac{y_i}{x_i} \ge L = \frac{y_L}{x_L}\)
```