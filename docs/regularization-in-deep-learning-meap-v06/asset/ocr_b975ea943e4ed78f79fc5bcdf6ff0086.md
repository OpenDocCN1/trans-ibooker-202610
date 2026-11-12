# 4.1.2 Shallow versus deep neural network

When dealing with a shallow network, i.e., a neural network with one hidden layer followed by an output layer, the tuning knob on the model flexibility is limited to the number of nodes/neurons in the hidden layer. In order to create a model with a specific number of breakpoints or linear segments in the final output, we need to add the same number of nodes in the hidden layer. These hidden neurons serve as the basis functions to be linearly combined to generate the final output. In other words, the model complexity grows linearly with the number of nodes in the hidden layer.

However, a deep neural network requires much fewer nodes per layer to reach the same level of complexity. Specifically, the model complexity (i.e., number of breakpoints or linear segments in the final output) grows exponentially with the number of hidden layers in the deep neural network. We will illustrate this point with an example of a hand-designed deep neural network with ReLU activation and observe how its complexity changes as the network become deeper.

Suppose we would like to fit the following function in each hidden layer. The resulting network is also referred to as the “sawtooth” network model. Each layer is the absolute value of the previous layer multiplied by two and shifted downwards by two, resulting in a final sawtooth-like fit. The relationship between the current layer and the previous layer can be expressed as the following:

$f_l(x)=2|f_{l-1}(x)|-2$

As usual, we have $f_0(x)=x$. There are two cases when taking the absolute value of input: if the input is positive, the output remains the same as the input; if the input is negative, the output is the negation of the input. We can therefore decompose the absolute function into two parts, each implemented via a ReLU activation function as follows:

$f_l(x)=2|f_{l-1}(x)|-2=2\text{ReLU}(f_{l-1}(x))+2\text{ReLU}(-f_{l-1}(x))-2$

Note that the weights are fixed in this example and no actual learning happens. We are interested in assessing the approximating power of the “sawtooth” network.

The two-neuron hidden layer can be defined via the following code snippet.

```
def layer(x):
    return 2*relu(x) + 2*relu(-x) - 2
```

Next, we define a function to build a neural network by sequentially stacking the layers based on the specified number of layers.

```
def multilayer(x, hidden_layers=1):
    f = x #A
    for _ in range(hidden_layers): #B
        f = layer(f)
    return f
```

#A First layer returns just the input itself
#B Sequentially pass the output of current layer as the input to the next layer