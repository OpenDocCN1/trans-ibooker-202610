In order to maximize the distance between the hyperplanes
\(\mathbf{w}^T \mathbf{x}_i + b = 1\) and \(\mathbf{w}^T \mathbf{x}_i + b = -1\)
we have to minimize the length of \(\mathbf{w} = (w_i)\):

```
D = \sqrt{\sum w_i^2} \tag{2.35}
```

subject to the constraints \(y_i(\mathbf{w}^T \mathbf{x}_i + b) \ge 1\). The vector \(\mathbf{w}\) and scale \(b\) define the classifier as

```
\mathbf{x} \rightarrow \operatorname{sign}\{\mathbf{w}^T \mathbf{x}_i + b\}
```

The vectors closest to the separating hyperplane are called *support vectors.*

# Decision Tree Classifier

The decision tree classifier technique (Rokach and Maimon 2008) is based on a logically based tree containing the test questions and conditions. The process uses a series of carefully selected questions about the test records of the attributes. Depending on the answer to a question, a well-selected follow-up question is asked; based on the answer, either a new question follows, or the process terminates with a decision about the category the object belongs to. A typical everyday example is the series of questions a doctor asks a patient to make the right diagnosis.

# Naive Bayesian Classifier

The naive Bayesian classifier technique (Webb et al. 2005) is also based on \(m\) classes \(C_1, ..., C_m\). An attribute vector \(\mathbf{x}\) belongs to class \(C_i\) if and only if for the conditional probabilities,

```
P(C_i|\mathbf{x}) > P(C_j|\mathbf{x}) \tag{2.36}
```

for all \(j \ne i\). By the Bayesian theorem

```
P(C_i|\mathbf{x}) = P(\mathbf{x}|C_i)P(C_i)/P(\mathbf{x})
```

And since \(P(\mathbf{x})\) is independent of the classes, optimal class \(C_i\) is selected by maximizing

```
P(\mathbf{x}|C_i)P(C_i) \tag{2.37}
```

The value of \(P(C_i)\) is usually taken as the relative frequency of the sample vectors belonging to class \(C_i\). The naive Bayesian classifier assumes the class conditional independence as

```
P(\mathbf{x}|C_i) = \prod_k P(x_k|C_i)
```

where the components of vector \(\mathbf{x}\) are the \(x_k\) values.

# Hidden Markov Chains

Hidden Markov chains (Ghahrani 2001) are probabilistic extensions of finite Markov chains illustrated earlier in Example 2.1. The states are not known, but certain probability values are assigned to them in addition to the state transition matrix. The states cannot be directly observed, but observations are made for outputs each state can produce with certain probabilities. This type of Markovian model is called *hidden*: since the states are hidden, only their outputs can be observed.