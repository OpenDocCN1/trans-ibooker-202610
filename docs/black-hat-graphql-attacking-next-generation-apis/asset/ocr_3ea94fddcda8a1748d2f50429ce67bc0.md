# 3

# THE GRAPHQL ATTACK SURFACE

![](bbox=[0.16, 0.47, 0.32, 0.63])

In this chapter, we first explore GraphQL’s language and type system through the eyes of a hacker. Then we provide an overview of the common weaknesses in GraphQL.
We hope you have your imaginary black hat handy, because you’re about to learn how a feature can turn into a weakness, how a misconfiguration can turn into an information leak, and how implementation design flaws can lead to DoS opportunities.

# What Is an Attack Surface?

An *attack surface* is the sum of all possible attack vectors an adversary can use to compromise the confidentiality, integrity, and availability of a system. For example, imagine a physical building with a front door, a side door, and