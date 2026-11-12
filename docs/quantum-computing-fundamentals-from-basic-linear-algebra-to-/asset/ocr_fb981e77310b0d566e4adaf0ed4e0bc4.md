Together, a_i and b_i provide an index to four qubit states, shown in Equation 7.9.

```
|ψ_00⟩ = |0⟩
|ψ_10⟩ = |1⟩
|ψ_01⟩ = |+⟩ = 1/√2 |0⟩ + 1/√2 |1⟩
|ψ_11⟩ = |-⟩ = 1/√2 |0⟩ + 1/√2 |1⟩
```

# EQUATION 7.9 SARG04 Qubit States

Note that the bit b_i is what decides which basis a_i is encoded in. The sender sends the quantum state |ψ⟩ over a public channel to the recipient. The recipient receives a state ε_P that combines the noise of the channel ε as well as any potential interference from a third party attempting to measure the quantum state. The recipient then generates a string of bits the same length as b (from the sender) and uses those bits as a basis when measuring bits from the sender. For each qubit sent, the sender must choose one computational basis state and one Hadamard basis state so that the state of the qubit is one of these two.

To go further with the SARG04 protocol, we must first explore Hadamard basis states. The Hadamard transform (Hadamard transformation, also known as the Walsh-Hadamard transformation) plays an important role in quantum computing. This transform is often called the Hadamard gate, and we will see it again in subsequent chapters. It is a one-qubit rotation, mapping the qubit-basis states |0⟩ and |1⟩ to two superposition states with an equal weight of the computational basis states |0⟩ and |1⟩. Usually, the phases are chosen, such that Equation 7.10 is true.

```
|0⟩ + |1⟩      |0⟩ + |1⟩
------- <0| + ------- <1|
√2             √2
```

# EQUATION 7.10 Hadamard Transform

Many quantum algorithms use the Hadamard transform as an initial step, since it maps n qubits initialized with |0⟩ to a superposition of all 2n orthogonal states in the |0⟩, |1⟩ basis with equal weight.

Now we return to SARG04. The sender will then announce the two states chosen. The recipient knows that his state is one of the two sender states and can then determine if his measurement is consistent with either state. If it is consistent with either state, then the bit is invalid because he cannot distinguish which state. If it is consistent with only one of the two candidate states, then this is a valid bit. This allows the bits of a key to be determined, which leads to a set of k valid bits. The sensor can randomly choose k/2 bits, and the sender and receiver can determine if they agree, and thus have a shared key.