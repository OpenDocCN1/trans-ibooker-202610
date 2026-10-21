```
[*] Discovered GraphQL Engine: (Graphene)
[!] Attack Surface Matrix: https://github.com/nicholasaleks/graphql-threat-matrix/blob/master/implementations/graphene.md
[!] Technologies: Python
[!] Homepage: https://graphene-python.org
[*] Completed.
```

# Listing 4-25: The fingerprinting of a GraphQL server

The tool first checks whether the target is, in fact, a GraphQL server. It does so by sending a few queries and inspecting their responses against its own database of signatures. As you can see, it is able to discover a GraphQL server running on Graphene and provides us with an attack surface matrix link. The attack surface matrix is essentially knowledge about the security posture of the various GraphQL implementations that Graphw00f can fingerprint. Graphw00f uses the GraphQL Threat Matrix we discussed in Chapter 3 as its implementation security posture database.

Since we now know that DVGA runs Graphene, we need to analyze Graphene’s weaknesses to determine which attacks we can run against this specific implementation. Some implementations have been around longer than others. Thus, they are more mature, stable, and offer more security features than others. This is why knowing the backend implementation is an advantage when we hack a GraphQL target.

# Analyzing Results

Take a look at the attack surface threat matrix, which provides information about the implementation’s default behavior and the security controls available for it (for example, the settings that are enabled by default, the security controls that exist, and other useful features we can leverage for hacking purposes). Figure 4-9 shows the attack surface matrix for Graphene. You can also find it on GitHub at https://github.com/nicholasaleks/graphql-threat-matrix/blob/master/implementations/graphene.md.

![](bbox=[0.24, 0.68, 0.71, 0.89])

# Figure 4-9: Graphene’s attack surface matrix