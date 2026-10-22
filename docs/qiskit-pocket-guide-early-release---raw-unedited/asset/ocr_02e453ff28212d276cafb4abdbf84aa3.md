# NOTE

The set of gates expressed using Qiskit represents an abstraction for the actual gates implemented on a given quantum computer or simulator. Qiskit transpiles the gates into those implemented on the target platform, combining gates where possible to optimize the circuit.

# Using the barrier() method

The barrier() method takes as an optional argument the qubit wires on which to place a barrier. If no argument is supplied, a barrier is placed across all of the quantum wires. This method creates a Barrier instance (see “Barrier”).

Example 1-2 demonstrates using the barrier() method with and without arguments.

# Example 1-2. Example of using the barrier() method

```
qc = QuantumCircuit(2)
qc.h([0,1])
qc.barrier()
qc.x(0)
qc.x(0)
qc.s(1)
qc.barrier([1])
qc.s(1)
```

Figure 1-4 shows the resultant circuit.

![](bbox=[0.12, 0.67, 0.88, 0.84])