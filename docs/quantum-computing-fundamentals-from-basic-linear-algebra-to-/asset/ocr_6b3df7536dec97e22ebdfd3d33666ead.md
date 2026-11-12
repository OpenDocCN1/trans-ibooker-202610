\[\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}\]

These are usually represented with the σ notation, thus:

\(\sigma_1 = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}\)

\(\sigma_2 = \begin{bmatrix} 0 & -i \\ i & 0 \end{bmatrix}\)

\(\sigma_3 = \begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}\)

It should not surprise you that there are Pauli gates. In fact, there are three such gates: Pauli-X, Pauli-Y, and Pauli-Z. Each of these operates on a single qubit, transforming it.

The Pauli-X gate is the quantum equivalent of the classical NOT gate. It is the equivalent of a rotation around the x-axis of the Bloch sphere by π radians. It will transform |0⟩ to |1⟩, and vice versa. And as you can probably guess, the Pauli-X matrix represents it:

\(X = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}\)

The Pauli-Y gate is the equivalent of a rotation around the y-axis by π radians. It will transform |0⟩ to i|1⟩ and |1⟩ to -i|0⟩. It is represented by the Pauli-Y matrix:

\(Y = \begin{bmatrix} 0 & -i \\ i & 0 \end{bmatrix}\)

Finally, we come to the Pauli-Z gate. You can probably guess that it is the equivalent of rotating about the z-axis of the Bloch sphere. It is a special case of a phase shift gate. It leaves the |0⟩ basis state unchanged, but maps the basis state |1⟩ to -|1⟩. It is represented by the Pauli-Z matrix:

\(Z = \begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}\)

# ## 8.2.4 Swap Gates

Unlike the gates we previously discussed in this section, swap gates operate on two qubits. As the name suggests, they swap the qubits. It is represented by the matrix shown in Figure 8.4.