Then, use \(\{(\mathbf{x}_t, y_t)|t=i_1, \dots, i_{n_1}\}\) as the training subsample. On the other hand, for serially dependent data, one typically uses the first \(n_1\) observations as the training subsample.

In what follows, we briefly discuss the training of a feedforward \(P-M-K\) network, which has \(P\) input nodes, a single hidden layer with \(M\) nodes, and \(K\) output nodes. See Hastie et al. (2001). This network is also referred to as a \(P-M-K\) perceptron. Denote the input as \(\mathbf{x}_t=(x_{1t}, \dots, x_{Pt})'\), the output as \(\mathbf{y}_t=(y_{1t}, \dots, y_{Kt})'\), and the vector of outputs of the hidden layer as \(\mathbf{h}_t=(h_{1t}, \dots, h_{Mt})'\). We can write down the model of the network as follows:

\(h_{mt}=g(w_{m0}+\boldsymbol{w}'_m\mathbf{x}_t),\quad m=1, \dots, M,\) (8.4)

\(q_{vt}=\beta_{v0}+\boldsymbol{\beta}'_v\mathbf{h}_t,\quad v=1, \dots, K,\) (8.5)

\(y_{vt}\equiv f_v(\mathbf{x}_t)=g_v(\mathbf{q}_t),\quad v=1, \dots, K,\) (8.6)

where \(g(.)\) and \(g_v(.)\) are activation functions, \(w_{m0}\) and \(\boldsymbol{w}_m=(w_{m1}, \dots, w_{mP})'\) are the bias and weights associated with the \(m\)th neuron of the hidden layer, \(\beta_{v0}\) and \(\boldsymbol{\beta}_v=(\beta_{v1}, \dots, \beta_{vM})'\) are, respectively, the bias and weights of the \(v\)th output neuron, and \(\boldsymbol{q}_t=(q_{1t}, \dots, q_{Kt})'\). In statistical terms, the model can be explained as a nonlinear factor model, where the original \(P\) explanatory variables are combined to create \(M\) linear combinations or factors. Each factor is then transformed in a nonlinear way to create new explanatory variables, \(h_{mt}\), and the \(K\) response variables are functions of the linear combinations of these new variables \(h_{nt}\). Also the bias can be incorporated into the weights by redefining the input variables as \(\mathbf{x}_t=(1, x_{1t}, \dots, x_{Pt})'\) and the hidden nodes as \(\mathbf{h}_t=(1, h_{1t}, \dots, h_{Mt})'\). The function \(f_{v(.)}\) of Eq. (8.6) is used to denote the underlying function for the \(v\)th output variable as a function of \(\mathbf{x}_t\). For continuous dependent variables \(\mathbf{y}_t\), \(g_v(.)\) often assumes the linear function \(g_v(\mathbf{q}_t)=q_{vt}=\beta_{v0}+\boldsymbol{\beta}'_v\mathbf{h}_t\). For discrete variables such as classification, \(g_v(.)\) typically assumes a multi-logit model, i.e.

\(f(\mathbf{x}_t)=\arg \max_{1 \le v \le K} \hat{f}_v(\mathbf{x}_t),\) (8.7)

where \(\hat{f}_v(.)\) denotes the fitted value of \(f_v(.)\) in the training subsample and \(f(.)\) denotes the underlying model. In this particular instance, given \(g(.)\) and \(g_v(.)\), the network contains the following parameters:

\(\{(\boldsymbol{w}_{m0}, \boldsymbol{w}_m|m=1, \dots, M): M(P+1) \text{ parameters},\) (8.8)

\(\{(\beta_{v0}, \boldsymbol{\beta}_v|v=1, \dots, K): K(M+1) \text{ parameters}.\) (8.9)

Let \(\boldsymbol{\theta}\) be the collection of all parameters (biases and weights). For continuous dependent variables \(\mathbf{y}_t\), one often uses the squared error loss function

\(L(\boldsymbol{\theta})=\sum_{v=1}^K \sum_{t=1}^{n_1} [y_{vt}-f_v(\mathbf{x}_t)]^2,\) (8.10)

where \(n_1\) is the sample size of the training subsample. For classification, one often uses the cross-entropy (or deviance), defined in Eq. (8.3), to measure the goodness