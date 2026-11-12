# Lattice-based Cryptography

# Chapter Objectives

After reading this chapter and completing the review questions, you will be able to do the following:

+   - Understand the basis for lattice-based cryptography
+   - Demonstrate a working knowledge of specific algorithms
+   - Comprehend how these algorithms can be quantum resistant
+   - Appreciate the principles of lattice reduction attacks

In previous chapters, we covered the fundamentals of quantum computing. Chapter 12, “The Impact of Quantum Computing on Cryptography,” explored the impact of quantum computing on cryptography and cybersecurity. This chapter explores a potential answer to the impact of quantum computing on cryptography. Lattice-based algorithms have been proposed as quantum-resistant asymmetric algorithms.

Lattice-based algorithms are cryptographic algorithms predicated on mathematical problems related to lattices. Recall that the security of RSA is based on the computational difficulty of factoring an integer into its prime factors. Diffie-Hellman, and its variations, have security based on the computational difficulty of solving the discrete logarithm problems. The various elliptic curve–based algorithms depend on the difficulty of solving the discrete logarithm in reference to an elliptic curve and a point not on the curve. All of these currently used algorithms have security predicated on some mathematical problem. Lattice-based algorithms have security based on mathematical problems related to lattices.

Before we explore specific problems, then algorithms, it is important to understand matrices related to lattices. Presumably you recall the material from Chapter 1, “Introduction to Essential Linear Algebra.” If you are not completely clear on those topics, you should review that chapter before continuing with this one. Lattices are matrices and can be any number of dimensions, though for ease of presentation, most