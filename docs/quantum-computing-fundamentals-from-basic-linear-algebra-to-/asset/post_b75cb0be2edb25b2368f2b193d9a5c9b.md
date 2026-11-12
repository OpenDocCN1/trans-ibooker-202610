
Now let us return to the collapse of a wave function, which takes the superposition of possible eigenstates and collapses to a single eigenstate based on interaction with the environment. What is this interaction with the environment? This is one of the aspects of quantum physics that is often misunderstood by the general public. A common interaction with the environment is a measurement, which physicists often describe as an observation. This has led many to associate intelligent observation as a necessary condition for quantum physics, and thus all of reality. That is simply not an accurate depiction of what quantum physics teaches us.

What is termed an observation is actually an interaction with the environment. When a measurement is taken, that is an interaction that causes the wave function to collapse.

The fact that a measurement causes the wave function to collapse has substantial implications for quantum computing. When one measures a particle, one changes the state. As you will see in later chapters, particularly Chapter 8, “Quantum Architecture,” and Chapter 9, “Quantum Hardware,” this is something that quantum computing must account for.

The wave function can be expressed as a linear combination of the eigenstates (recall this is the physics term for eigenvectors you learned in Chapter 1) of an observable (position, momentum, spin, etc.). Using the bra-ket notation discussed previously, this means a wave function has a form such as you see in Equation 6.5.

```
| ψ ⟩ = Σᵢ cᵢ | φᵢ ⟩
```

### EQUATION 6.5 Wave Function

This is not as complex as it seems. The Greek letter psi (ψ) denotes the wave function. The Σ symbol is a summation of what is after it. The φi represents various possible quantum states. The i is to enumerate through those possible states, such as φ1, φ2, φ3, etc. The ci values (i.e., c1, c2, c3, etc.) are probability coefficients. The letter c is frequently used to denote these because they are represented by complex numbers.

Recall from Chapter 1 that if two vectors are both orthogonal (i.e., perpendicular to each other) and have a unit length (length 1), the vectors are said to be orthonormal. The bra-ket ⟨φi | φj⟩ forms an orthonormal eigenvector basis. This is often written as follows:

```
⟨φi | φj⟩ = δij.
```

The symbol δ is the Kronecker delta, which is a function of two variables. If the variables are equal, the function result is 1. If they are not equal, the function result is 0. This is usually defined as shown in Equation 6.6.

```
δij = { 0 if i ≠ j, 1 if i = j }
```

### EQUATION 6.6 Kronecker Delta

Now let us discuss the actual process of the wave collapse. Remember that for any observable, the wave function is some linear combination of the eigenbasis before the collapse. When there is some environmental interaction, such as a measurement of the observable, the function collapses to just one of the base's eigenstates. This can be described in the following rather simple formula:

```
| ψ ⟩ → | φk ⟩
```

But which state will it collapse to? That is the issue with quantum mechanics being probabilistic. We can say that it will collapse to a particular eigenstate | φk ⟩ with the Born probability (recall we discussed this earlier in this chapter) P_k = | c_k |^2. The value c_k is the probability amplitude for that specific eigenstate. After the measurement, all the other possible eigenstates that are not k have collapsed to 0 (put a bit more mathematically, c_{i≠k} = 0).

Measurement has been discussed as one type of interaction with the environment. One of the challenges for quantum computing is that this is not the only type of interaction. Particles interact with other particles. In fact, such things as cosmic rays can interact with quantum states of particles. This is one reason that decoherence is such a problem for quantum computing.

## 6.1.4 Schrödinger's Equation

The Schrödinger equation is quite important in quantum physics. It describes the wave function of a quantum system. This equation was published by Erwin Schrödinger in 1926 and resulted in his earning the Nobel Prize in Physics in 1933. First, let us examine the equation itself and ensure you have a general grasp of it; then we can discuss more of its implications. There are various ways to present this equation; we will first examine the time-dependent version. You can see this in Equation 6.7.

```
i\hbar \frac{\partial}{\partial t} | \psi (t) \rangle = \hat{H} | \psi (t) \rangle
```

### EQUATION 6.7 Schrödinger Equation

Don't let this overwhelm you. All of the symbols used have already been discussed, and I will discuss them again here to refresh your memory.

Given that we are discussing a time-dependent version of the Schrödinger equation, it should be clear to most readers that the t represents time. Remember that the ∂ symbol indicates a partial derivative. So, we can see in the denominator that there is a partial derivative with respect to time. The ℏ, you will recall from Chapter 3 and from earlier in this chapter, is the reduced Planck constant, which is the Planck constant h (6.626 × 10^-34 J·s) / 2π. The ψ symbol we saw earlier in this chapter. You may also recall that the symbol Ĥ denotes the Hamiltonian operator, which is the total energy of the particles in a system.

Before we examine the implications of the Schrödinger equation, let us first examine another form of the equation. You can see this in Equation 6.8.

```
\frac{\partial^2 \psi}{\partial x^2} + \frac{8\pi^2 m}{h^2} (E-V)\psi = 0
```

### EQUATION 6.8 Schrödinger (Another Form)

You already know that the ∂ symbol indicates a partial derivative. The 2 superposed above it means this is a second derivative (i.e., a derivative of a derivative). For those readers who don’t have a solid calculus background, or who don’t recall their calculus, a second derivative is actually common. A first derivative tells you the rate of change for some function. A second derivative tells you the rate of change for that rate of change that you found in the first derivative. Probably the most common example is acceleration. Speed is the change in position with respect to time. That is the first derivative. Acceleration is the change in speed, which is a second derivative. The ψ symbol denotes the wave function, which you should be getting quite familiar with by now. Another symbol you are familiar with is the h, for Planck’s constant. Note in this form of the Schrödinger equation that it is the Planck constant, not the reduced Planck constant. The E is the kinetic energy, and the V is the potential energy of the system. The x is the position.

Remember that in the subatomic world, we have the issue of wave-particle duality. The Schrödinger equation allows us to calculate how the wave function changes in time.

## 6.2 Quantum Decoherence

Quantum decoherence is a very important topic and is, in fact, critical for quantum computing. Decoherence is directly related to the previous section on wave functions. Recall that a wave function is a mathematical representation of the state of a quantum system. As long as there exists a definite phase relation between the states, that system is coherent. Also, recall that interactions with the environment cause a wave function to collapse. If one could absolutely isolate a quantum system so that it had no interaction at all with any environment, it would maintain coherence indefinitely. However, only by interacting with the environment can it be measured; thus, data can be extracted.

What does it mean to have a definite phase relation between states? First, we must examine the concept of phase space, which is a concept from dynamical system theory. It is a space in which all the possible states of the system are represented. Each state corresponds to a unique point in the phase space. Each parameter of the system represents a degree of freedom. In turn, each degree of freedom is represented as an axis of a multidimensional space. If you have a one-dimensional system, it is a phase line. Two-dimensional systems are phase planes.

Two values, p and q, play an important role in phase space. In classical mechanics, the p is usually momentum and the q the position. Now, in quantum mechanics, this phase space is a Hilbert space. Thus, the p and q are Hermitian operators in that Hilbert space. While momentum and position are the most common observables, and are most often used to define phase space, there are other observables such as angular momentum and spin.
