Example 1-11 contains an example of using the from_qasm_file() method.

# Example 1-11. Example of using the from_qasm_file() method

```
new_qc = QuantumCircuit.from_qasm_file("file.qasm")
```

# Using the from_qasm_str() method

The from_qasm_str() method returns a new circuit from a string that contains an OpenQASM program. Example 1-12 contains an example of using the from_qasm_str() method, and Figure 1-12 shows a drawing of the resultant circuit.

# Example 1-12. Example of using the from_qasm_str() method

```
qasm_str = """
OPENQASM 2.0;
include "qelib1.inc";
qreg q[2];
creg c[2];
h q[0];
cx q[0],q[1];
measure q[0] -> c[0];
measure q[1] -> c[1];
"""
new_qc = QuantumCircuit.from_qasm_str(qasm_str)
new_qc.draw()
```

![](bbox=[0.12, 0.6, 0.88, 0.78])

Figure 1-12. Example circuit resulting from the from_qasm_str() method

# Using the initialize() method