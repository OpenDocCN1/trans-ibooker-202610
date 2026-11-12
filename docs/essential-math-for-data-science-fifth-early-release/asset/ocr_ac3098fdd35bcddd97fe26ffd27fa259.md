that vector through the ReLU function and it will give us \(A_1\). Because all the values are positive, it should not have an impact.

\(A_1 = ReLU (Z_1)\)

\(A_1 = \begin{bmatrix} ReLU(1. 36054964190909) \\ ReLU(2. 15471757888247) \\ ReLU(0. 7195543933391768) \end{bmatrix} = \begin{bmatrix} 1. 36054964190909 \\ 2. 15471757888247 \\ 0. 7195543933391768 \end{bmatrix}\)

Now let's take that hidden layer output \(A_1\) and pass it through the final layer to get \(Z_2\) and then \(A_2\). \(A_1\) becomes the input into the output layer.

\(Z_2 = W_{output}A_1 + B_{output}\)

\(Z_2 = [0. 82652072 \quad 0. 3078159 \quad 0. 93095565] \begin{bmatrix} 1. 36054964190909 \\ 2. 15471757888247 \\ 0. 7195543933391768 \end{bmatrix} + [0. 58018555]\)

\(Z_2 = [2. 45765202842636] + [0. 58018555]\)

\(Z_2 = [3. 03783757842636]\)

Finally pass this single value in \(Z_2\) through the activation function to get \(A_2\). This will produce a prediction of approximately \(0 . 95425\).

\(A_2 = logistic (Z_2)\)

\(A_2 = logistic ([3. 0378364795204])\)

\(A_2 = logistic ([3. 0378364795204])\)

\(A_2 = 0. 954254478103241\)

That executes our entire neural network, although we have not trained it yet. But take a moment to appreciate that we have taken all these input values, weights, biases, and nonlinear functions and turned it all into a single value that will provide a prediction.

Again, \(A_2\) is the final output that makes a prediction whether that background color need a light (1) or dark (1) font. Even though our weights and biases have not been optimized yet, let's calculate our accuracy as shown in Example 7-6. Take the test dataset X\_test, transpose it, and pass it through the `forward_prop()` function but only grab the \(A_2\) vector with the predictions for each test color. Then compare the predictions to the actuals, and calculate the percentage of correct predictions.

# Example 7-6. Calculating Accuracy