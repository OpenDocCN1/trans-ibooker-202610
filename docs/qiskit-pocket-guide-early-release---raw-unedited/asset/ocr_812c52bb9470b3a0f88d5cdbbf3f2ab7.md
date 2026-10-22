Example 1-5 contains an example of using the `append()` method, and Figure 1-7 shows a drawing of the resultant circuit.

# Example 1-5. Example of using the append() method

```
qc = QuantumCircuit(2)
qc.h(1)
cx_gate = CXGate()
qc.append(cx_gate, [1,0])
qc.draw()
```

![](bbox=[0.12, 0.28, 0.88, 0.46])

Figure 1-7. Example circuit resulting from the append() method

NOTE
The CXGate class (see “CXGate”) used in the example is one of the gates defined in the qiskit.circuit.library package.

# Using the bind_parameters() method

The bind_parameters() method binds parameters (see “Creating a Parameter Instance”) to a quantum circuit. Example 1-6 contains an example of creating a circuit in which there are three parameterized phase gates. Figure 1-8 shows a drawing of the circuit.

# Example 1-6. Creating a parameterized circuit

```
theta1 = Parameter('θ1')
theta2 = Parameter('θ2')
theta3 = Parameter('θ3')

qc = QuantumCircuit(3)
```