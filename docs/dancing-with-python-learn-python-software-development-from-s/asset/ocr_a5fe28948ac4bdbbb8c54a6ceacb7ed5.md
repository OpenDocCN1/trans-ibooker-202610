# The CNOT quantum gate

In the quantum case, CNOT is more correctly called the controlled-X or CX gate.

![](bbox=[0.35, 0.18, 0.68, 0.23])

![](bbox=[0.35, 0.26, 0.68, 0.32])

X is applied to the second qubit if the first qubit’s quantum state is |1⟩. If the state is |0⟩, the second qubit’s state is left unchanged.

The top gate application says

CNOT (|0⟩ ⊗ (a |0⟩ + b |1⟩)) = |0⟩ ⊗ (a |0⟩ + b |1⟩) = a |00⟩ + b |01⟩.

The bottom says

CNOT (|1⟩ ⊗ (a |0⟩ + b |1⟩))
= |1⟩ ⊗ X (a |0⟩ + b |1⟩)
= |1⟩ ⊗ (b |0⟩ + a |1⟩)
= b |10⟩ + a |01⟩.

I cover the CNOT / CX gate in section 8.3.3 of Dancing with Qubits. [D.W.Q]

# The SWAP quantum gate

The SWAP interchanges the quantum states of two qubits.

![](bbox=[0.35, 0.78, 0.68, 0.84])