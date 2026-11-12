function similar to a kernel function in the statistical literature, to perform network training. Kalchbrenner et al. (2014) apply CNN for modeling sentences.

**Recursive NN**: A recursive network is a deep NN created by applying the same set of weights recursively over a structured input to produce a structured prediction. It has been applied for speech recognition as a sentence is the sum of the words and, therefore, has structure. Each word is represented by a vector and a binary tree is formed. Recursive NNs operate on any hierarchical structure, combining the information in two nodes (children) into a new one (parent) and employ tree-type structure to process information.

**Recurrent neural network (RNN)**: These networks can be considered a particular case of recursive NN representations operating on the linear progression of time, combining the previous time step and a hidden representation to obtain the output of the current time step. RNNs are designed to process sequential data and are especially useful for time series analysis. They were first created for data that arrive with some kind of sequential structure, as handwriting and speech recognition. See, for instance, Mikolov et al. (2010). RNN have been applied to forecasting vectors of time series assuming a nonlinear autoregressive model with exogenous variables (NARX). See Menezes and Barreto (2008). The issue of regularization of RNN is studied by Zaremba et al. (2015), and properties of RNN have been investigated in the literature. See, for instance, Hammer et al. (2005) and Hammer (2007). Since RNN focuses on dynamic dependence, we provide further details of them in the next section.

# 8.3.2 Recurrent NN

RNNs are a family of NNs designed to process sequential data. They are similar to the dynamic system

```
s_t = f(s_{t-1}, x_t, \theta),
```

where $s_t \in \mathbb{R}^d$ denotes a state vector at time $t$, $x_t \in \mathbb{R}^k$ is the input at time $t$, $\theta \in \mathbb{R}^p$ is a set of parameters, and $f(.)$ denotes the transition from state $s_{t-1}$ to $s_t$ with new data $x_t$. A special feature of the dynamic system is that the parameter vector $\theta$ is time-invariant. That is, the same values are shared by all time indexes $t$. Given the input data, $\{x_1, ..., x_n\}$, let $h_t \in \mathbb{R}^d$ be the output of the hidden layer, where $h_0 = \mathbf{0}$. Then, a simple RNN can be symbolically described as

```
s_t = b + W h_{t-1} + U x_t, \qquad (8.24)

h_t = g(s_t), \qquad (8.25)

\hat{y}_t = g_y(c + V h_t), \qquad (8.26)
```

where $g(.)$ and $g_y(.)$ are activation functions such as tanh, $b$ and $c$ are the bias terms, and $W, U$ and $V$ are parameter matrices and $\hat{y}_t \in \mathbb{R}^m$. In the literature, the RNN in Eqs. (8.24)–(8.26) is referred to as the Elman network; see Elman (1990). Figure 8.8 is a representation of the RNN. At each time $t$, the available information is the input $x_t$ and the vector of memory, $h_{t-1}$, from $t-1$. These two vectors are combined linearly to form the state vector $s_t$ at time $t$. The new vector of memory is a nonlinear function