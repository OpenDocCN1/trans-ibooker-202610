Now let us return to the collapse of a wave function, which takes the superposition of possible eigenstates and collapses to a single eigenstate based on interaction with the environment. What is this interaction with the environment? This is one of the aspects of quantum physics that is often misunderstood by the general public. A common interaction with the environment is a measurement, which physicists often describe as an observation. This has led many to associate intelligent observation as a necessary condition for quantum physics, and thus all of reality. That is simply not an accurate depiction of what quantum physics teaches us.

What is termed an observation is actually an interaction with the environment. When a measurement is taken, that is an interaction that causes the wave function to collapse.

The fact that a measurement causes the wave function to collapse has substantial implications for quantum computing. When one measures a particle, one changes the state. As you will see in later chapters, particularly Chapter 8, “Quantum Architecture,” and Chapter 9, “Quantum Hardware,” this is something that quantum computing must account for.

The wave function can be expressed as a linear combination of the eigenstates (recall this is the physics term for eigenvectors you learned in Chapter 1) of an observable (position, momentum, spin, etc.). Using the bra-ket notation discussed previously, this means a wave function has a form such as you see in Equation 6.5.

```
ψ >= Σi ci |φi
```

# EQUATION 6.5 Wave Function

This is not as complex as it seems. The Greek letter psi (ψ) denotes the wave function. The Σ symbol is a summation of what is after it. The φi represents various possible quantum states. The i is to enumerate through those possible states, such as φ1, φ2, φ3, etc. The ci values (i.e., c1, c2, c3, etc.) are probability coefficients. The letter c is frequently used to denote these because they are represented by complex numbers.

Recall from Chapter 1 that if two vectors are both orthogonal (i.e., perpendicular to each other) and have a unit length (length 1), the vectors are said to be orthonormal. The bra-ket ⟨φi | φj⟩ forms an orthonormal eigenvector basis. This is often written as follows:

```
⟨φi | φj⟩ = δij.
```

The symbol δ is the Kronecker delta, which is a function of two variables. If the variables are equal, the function result is 1. If they are not equal, the function result is 0. This is usually defined as shown in Equation 6.6.

```
δij = { 0 if i ≠ j, 1 if i = j.
```

# EQUATION 6.6 Kronecker Delta