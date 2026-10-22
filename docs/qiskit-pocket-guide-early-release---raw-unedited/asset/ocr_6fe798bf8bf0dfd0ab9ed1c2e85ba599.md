![](bbox=[0.22, 0.07, 0.78, 0.22])

# DCXGate

The `DCXGate` class creates a double-CNOT gate. This is a two-qubit gate that has two **CNOT** gates with their control-qubits on different wires.

Signature: Appearance:

![](bbox=[0.43, 0.4, 0.67, 0.53])

```
DCXGate()
```

# iSwapGate

The `iSwapGate` class swaps the qubit states of two quantum wires. It also changes the phase of |01⟩ and |10⟩ amplitudes by i.

Signature:

Appearance:

![](bbox=[0.62, 0.71, 0.82, 0.83])

```
iSwapGate(label=None, ctrl_state=None)
```

# MCPhaseGate