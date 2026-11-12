
For two variables or two messages that are independent, their *joint entropy* is the sum of the two entropies. That formula is shown in Equation 5.6.

```
H(X,Y) = -\sum_{x,y} p(x,y) \log p(x,y)
```

### EQUATION 5.6 Joint Entropy

If the two variables are not independent, but rather variable Y depends on variable X, then instead of joint entropy, you have *conditional entropy*. That formula is shown in Equation 5.7.

```
H(X|Y) = -\sum_{x,y} p(x,y) \log p(x|y)
```

### EQUATION 5.7 Conditional Entropy

*Mutual information* is a term for the amount of information that can be obtained about one random variable by observing another. The mutual information for X relative to Y is shown in Equation 5.8.

```
I(X; Y) = \sum_{x,y} p(x,y) \log \frac{p(x,y)}{p(x)p(y)}
```

### EQUATION 5.8 Mutual Information

Differential entropy, sometimes called *continuous entropy*, is an extension of Shannon entropy to measure the average “surprisal” of a random variable to continuous probability distributions. Remember that the information is the level of uncertainty or “surprise” in a message. Unfortunately, Shannon’s formula for this was actually not correct. Shannon’s formula was adjusted to produce the limiting density of discrete points (LDDP). This formula is given in Equation 5.9.

```
\lim_{N \to \infty} H_N(X) = \log(N) - \int p(x) \log \frac{p(x)}{m(x)} dx.
```

### EQUATION 5.9 Limiting Density of Discrete Points

This formula requires a bit of basic calculus. If you will recall in Chapter 4, “Fundamental Computer Science for Quantum Computing,” we gave a brief description of integrals, and any reader with even a single semester of calculus will recognize the limit.

As was discussed earlier, information theory has developed since Claude Shannon. Another type of information is Fisher information, which measures the amount of information in an observable variable X about an unknown parameter θ of some distribution that models X.

**Note**

Fisher information is a bit more mathematically challenging than what we have covered in this chapter. The interested reader can get more information on Fisher information from https://arxiv.org/pdf/1705.01064.pdf.

It should be clear that the topic of information theory and information entropy is a rather broad topic. There are a number of formulas for quantifying information in a range of scenarios. It is important that you have a basic working knowledge of information theory in order to proceed to later chapters that will discuss quantum information theory.

Table 5.1 summarizes the formulas for information theory presented in this section.

| Concept | Formula |
|---|---|
| Shannon information entropy | \( H = -\sum_i p_i \log_2 (p_i) \) |
| Joint entropy | \( H(X,Y) = -\sum_{x,y} p(x,y) \log p(x,y) \) |
| Conditional entropy | \( H(X|Y) = -\sum_{x,y} p(x,y) \log p(x|y) \) |
| Mutual information | \( I(X; Y) = \sum_{x,y} p(x,y) \log \frac{p(x,y)}{p(x)p(y)} \) |
| Differential entropy | \( \lim_{N \to \infty} H_N(X) = \log(N) - \int p(x) \log \frac{p(x)}{m(x)} dx \) |

The interested reader can find more on information theory at https://d2l.ai/chapter_appendix-mathematics-for-deep-learning/information-theory.html.

## 5.3.4 Information Diversity

There are different metrics utilized as specific diversity indices for particular applications. One such diversity metric is the Shannon-Weaver index. This is sometimes referred to as the Shannon diversity index. The eponymously named Shannon diversity index was posited by Claude Shannon. Shannon used this metric to quantify the informational content in a given string of text. Shannon was most interested in electronic communications transmissions. When working with information theory, the information content of a message is referred to as the *information entropy*. The formula is shown in Equation 5.10.

$H' = -\sum_{i=1}^n p_i \ln p_i$

### EQUATION 5.10 Shannon-Weaver Index

In Equation 5.10, $p_i$ denotes the proportion of symbols that are part of the class of the $i$th class of symbol in the string. H' symbolizes the Shannon diversity index (also referred to as the Shannon-Weaver diversity index). This is shown in Equation 5.11.

$H = -\sum_{i=1}^S p_i \ln p_i$

### EQUATION 5.11 Shannon-Weaver Index, Form 2

In Equation 5.11, S is the number of individual species found in the community being studied. The $p_i$ is still the proportion of symbols that are part of the $i$th category of the symbol in the text string of interest. There are several variants of the Simpson index. Four examples of such variations are the Berger-Parker index, the Inverse Simpson index, the Dominance index, and the Gini-Simpson index.

Hartley entropy was introduced by Ralph Hartley in 1928 and thus predates Shannon's work on information theory. This is often simply called the Hartley function. If a sample from a finite set A uniformly is picked at random, the information revealed after the outcome is known is given by the Hartley function, shown in Equation 5.12.

$H_0(A) := \log_b |A|$

### EQUATION 5.12 Hartley Entropy

In Equation 5.12, $|A|$ denotes the cardinality of A. If the base of the logarithm is 2, then the unit of uncertainty is now referred to as the Shannon. If it is the natural logarithm, then the unit is the nat. Hartley used a base 10 logarithm, and with this base, the unit of information is called the Hartley in his honor. Like the Shannon-Weaver and Simpson indices, the Hartley entropy provides insight regarding the diversity in a given dataset.

Related to some of the previous entropy values discussed so far is the Rényi entropy, which generalizes the Hartley entropy, the Shannon entropy, the collision entropy, and the min-entropy, as shown in Equation 5.13.

$H_\alpha(X) = \frac{1}{1-\alpha} \log \left( \sum_{i=1}^n p_i^\alpha \right)$

### EQUATION 5.13 Rényi Entropy

In Equation 5.13, X is a discrete random variable with possible outcomes (1, 2, ..., n) and corresponding probabilities, p_i, for i = 1 to n. Because this metric is generally used in information theory, the logarithm is typically base 2, and the order, alpha, is 0 < alpha < 1.

Collision entropy is basically a special case of the Rényi entropy, where alpha = 0 and where X and Y are independent and are identically distributed. The formula for collision entropy is shown in Equation 5.14.

```
H_2(X) = -\log P(X = Y)
```

### EQUATION 5.14 Collision Entropy

Min-entropy is the smallest of the Rényi entropies. The min-entropy will always be less than or equal to the standard Shannon entropy discussed earlier in this section.

The Rényi entropy is a measure of the diversity in a dataset. The Rényi entropy is also important in quantum information, where it can be used as a measure of entanglement. As with other diversity metrics, this can be useful in fully understanding the data flow between two or more network nodes.

Another important measure of information entropy is Gibbs entropy. This will be directly used in the following section on quantum information. The Gibbs entropy characterizes the state of a system by the distribution of the microstates. The formula is actually rather simple and is shown in Equation 5.15.

```
S = -k_B \sum p_i \ln p_i
```

### EQUATION 5.15 Gibbs Entropy Formula

In Equation 5.15, many of the symbols should already be familiar to you. A new symbol is $k_B$. This is the Boltzmann constant. The Boltzmann constant relates to the average kinetic energy of particles in a gas. In this case, it is related to the particles in a given system.

## 5.4 Quantum Information

Now that you have been introduced to information theory, it is time to discuss a few aspects of quantum information theory. We will address some aspects of quantum information theory in this section. Others will be introduced as appropriate when we discuss quantum algorithms and related topics in later chapters, including the next chapter.

We have not yet delved into qubits. We will discuss qubits in more detail in Chapter 6, “Basic Quantum Theory,” Chapter 7, “Quantum Entanglement and QKD,” and Chapter 10, “Quantum Algorithms.” For the time being, let us just say that these are the quantum versions of bits, and that these qubits have a probabilistic, not deterministic, outcome. The no-teleportation theory simply states that an arbitrary
