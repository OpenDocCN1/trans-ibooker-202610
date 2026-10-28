depicted in Figure 2.2b. Therefore, analogous to deterministic signal processing techniques, the covariance and power spectra enable the powerful methods of linear systems theory to be applied to extract useful signal information for noisy data.

In this chapter, we are concerned with the development of analysis methods for random signals that will prove useful in applications to follow.

# 2.2 Discrete Random Signals

A random signal or equivalently a stochastic process finds its roots in probability theory. The idea of an underlying probability or sample space is essential to the treatment of random signals. Recall that a probability/sample space, Ξ, is a collection of samples or experimental outcomes, ξ, which are elements of that space. Certain subsets of Ξ or collections of outcomes are the events. For example, if we flip a fair coin, then the sample space consists of the set of outcomes, Ξ = {H, T} and the events, {∅, {H}, {T}}. When we attempt to specify how likely a particular event will occur during a trial or experiment, then we define the notion of probability. In terms of our probability space, we define a probability function, Pr(·), as a function defined on a class of events, which assigns a number to the likelihood of a particular event occurring during an experiment. We must constrain the class of events on which the probability function is defined so that set operations performed produce sets that are still events. Therefore, the class of sets satisfying this constraint is called a field (Borel). If we have a space, Ξ, a field, B, and a probability function, Pr(·), we can precisely define an experiment as the triple, {Ξ, B, Pr}. Stochastic processes can be thought of as a collection of indexed random variables. More formerly, a random signal or stochastic process is a two-dimensional function of t and ξ:

X(t, ξ)      ξ∈Ξ,    t ∈ T
(2.6)

where T is a set of index parameters (continuous or discrete) and Ξ is the sample space.

So we see that a discrete random signal can be precisely characterized by

x_i(t) := X(t, ξ_i)
(2.7)

A realization of a stochastic process and in fact each distinct value of t can be interpreted as a random variable. Thus, we can consider a stochastic process, simply a sequence of ordered random variables. A collection of realizations of a stochastic process is called an ensemble.

A common representation of a random process is a random walk given in the following example.