We insert a final CNOT.

```
circuit.cx(0, 1)
```

<qiskit.circuit.instructionset.InstructionSet at 0x1cf9e8be880>

```
circuit.draw(**draw_kwargs)
```

![](bbox=[0.35, 0.24, 0.68, 0.31])

# Exercise 9.42

The above circuit implements one of the 2-qubit gates we have seen before. Which is it and why?

The qubits q₀ and q₁ are initialized to |0⟩ by the IBM Quantum hardware, but I want to set q₁ to |1⟩. Here is a short circuit that is explicit about the values:

```
initialization_circuit = QuantumCircuit(2)
initialization_circuit.id(0)
initialization_circuit.x(1)
initialization_circuit.barrier()
```

<qiskit.circuit.instructionset.InstructionSet at 0x1cf9eaf6fd0>

![](bbox=[0.42, 0.68, 0.68, 0.75])

I inserted the barrier to separate the initialization phase from what follows.

Now I “compose” the circuits together to make a new circuit.

```
new_circuit = initialization_circuit.compose(circuit)
new_circuit.measure_all()
```

```
new_circuit.draw(**draw_kwargs)
```