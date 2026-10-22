![](bbox=[0.45, 0.12, 0.62, 0.17])

Figure 1-14. Example circuit resulting from the using the reset() method

# Using the qasm() method

The qasm() method returns an OpenQASM program that represents the quantum circuit. Example 1-15 contains an example of using the qasm() method, and Example 1-16 shows the resultant OpenQASM program.

# Example 1-15. Example of using the qasm() method

```
qc = QuantumCircuit(2, 2)
qc.h(0)
qc.cx(0, 1)
qasm_str = qc.qasm()
print(qasm_str)
```

# Example 1-16. OpenQASM program resulting from the using the qasm() method

```
OPENQASM 2.0;
include "qelib1.inc";
qreg q[2];
creg c[2];
h q[0];
cx q[0],q[1];
```

# Using the to_gate() method

The to_gate() method creates a custom gate (see “The Gate Class”) from a quantum circuit. Example 1-17 contains an example of creating a circuit that will be converted to a gate, and Figure 1-15 shows a drawing of the circuit.