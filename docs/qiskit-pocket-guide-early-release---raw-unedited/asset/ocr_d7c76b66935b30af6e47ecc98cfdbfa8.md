![](bbox=[0.13, 0.11, 0.87, 0.28])

Figure 1-6. Example circuit using the measure_all() method

Notice that the `measure_all()` method created three classical wires, and added a barrier to the circuit, before appending the measurement operations.

# Obtaining information about a quantum circuit

Methods commonly used to obtain information about a quantum circuit include `depth()`, `size()`, and `width()`. These are listed in Table 1-3. Note that variable `qc` refers to an instance of QuantumCircuit.

Table 1-3. Methods commonly used to obtain information about a quantum circuit

| Names | Example | Notes |
|---|---|---|
| `depth` | `qc.depth()` | Returns the depth (critical path) of a circuit if directives such as barrier were removed |
| `size` | `qc.size()` | Returns the total number of gate operations in a circuit |
| `width` | `qc.width()` | Return the sum of qubits wires and classical wires in a circuit |