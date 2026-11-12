There is also the controlled-U gate. This gate also operates on two qubits, and the first qubit is a control (thus the name). The mapping of the controlled-U gate is shown here:

```
|00> -> |00>
|01> -> |01>
```

Note that if the first qubit is zero, nothing is changed.

There are more complex gates, such as this one:

```
|10> -> |1> ⊗ U|0> = |1> ⊗ (u_00|0> + u_10|1>)
```

Before continuing, we must examine what this means, because some of it might seem quite odd. The U stands for one of the Pauli matrices (σ_x σ_y σ_z). If it is the Pauli-Z matrix, then it is a controlled Z gate. Thus, u00 means |00> input into a particular Pauli matrix. Now with that explanation, we can complete our description.

We now show Pauli matrices used in a more complex gate.

```
|11> -> |1> ⊗ U|1> = |1> ⊗ (u_01 |0> + u_11 |1>)
```

As you can see, this is a bit more of a complex gate than some of the others we have discussed. However, you can view it as a controlled Pauli gate, if that helps you to understand it.

The quantum CNOT (controlled NOT) gate operates much like what you see in Figure 8.5.

| Input | Output |
|---|---|
| |00> | |00> |
| |01> | |01> |
| |10> | |11> |
| |11> | |10> |

FIGURE 8.5 Quantum CNOT

# 8.2.8 Ising Gates

Ising gates are very interesting gates. There are actually three of them: XX, YY, and ZZ gates. These are in fact often called Ising coupling gates. These are named after Ernst Ising, who developed the eponymously named Ising model. The Ising model represents the magnetic dipole moments of atomic spins.

Ising gates are two qubit gates that are implemented by some quantum computers, particularly those that operate with trapped ions. We will be discussing more hardware-specific issues, such as trapped ions, in Chapter 9. These gates are more complex than the gates we have previously discussed in