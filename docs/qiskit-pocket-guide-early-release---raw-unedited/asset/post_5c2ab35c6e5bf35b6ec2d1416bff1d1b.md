
![](img/4a3b8ee6681ceda653aea587113057e9_46_0.png)

## C3SXGate

The C3SXGate class creates a four-qubit gate that has a square root of X gate and three control qubits.

Signature:

Appearance:

```
C3SXGate(label=None, ctrl_state=None, *,
angle=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_46_1.png)

## C4XGate

The C4XGate class creates a five-qubit gate that has an X gate and four control qubits.

Signature:

Appearance:

```
C4XGate(label=None, ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_47_0.png)

## CCXGate

The `CCXGate` class creates a three-qubit gate that has an **X** gate and two control qubits. This is also known as a Toffoli gate.

Signature:

Appearance:

```
CCXGate(label=None, ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_47_1.png)

## CHGate

The `CHGate` class creates a controlled-Hadamard gate, applying the Hadamard according to the control qubit state.

Signature:

Appearance:

```
CHGate(label=None, ctrl_state=None)
```

## CPhaseGate

The `CPhaseGate` class creates a controlled-Phase gate, applying the `PhaseGate` according to the control qubit state.

Signature:

Appearance:

```
CPhaseGate(theta, label=None,
ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_48_0.png)

## CRXGate

The `CRXGate` class creates a controlled-RX gate, applying the RX according to the control qubit state.

Signature:

Appearance:

```
CRXGate(theta, label=None, ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_48_1.png)

## CRYGate

![](img/4a3b8ee6681ceda653aea587113057e9_48_2.png)

The `CRYGate` class creates a controlled-RY gate, applying the `RY` according to the control qubit state.

Signature:

Appearance:

```
CRYGate(theta, label=None, ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_49_0.png)

## CRZGate

The `CRZGate` class creates a controlled-RZ gate, applying the `RZ` according to the control qubit state.

Signature:

Appearance:

```
CRZGate(theta, label=None, ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_49_1.png)

## CSwapGate

The `CSwapGate` class creates a three-qubit gate whose **Swap** gate is applied according to the control qubit state.

Signature:

Appearance:

```
CSwapGate(label=None, ctrl_state=None)
```

## CSXGate

The `CSXGate` class creates a controlled-SX (square root of X) gate, applying the √X gate according to the control qubit state.

Signature:

Appearance:

```
CSXGate(label=None, ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_50_0.png)

![](img/4a3b8ee6681ceda653aea587113057e9_50_1.png)

## CUGate

The `CUGate` class creates a controlled-U gate, applying the U gate including a global phase argument, according to the control qubit state.

Signature:

Appearance:

```
CUGate(theta, phi, lam, gamma, label=None,
ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_50_2.png)

## CXGate

The `CXGate` class creates a controlled-X gate, applying the **X** gate according to the control qubit state.

Signature:

Appearance:

```
CXGate(label=None, ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_51_0.png)

## CYGate

The `CYGate` class creates a controlled-Y gate, applying the **Y** gate according to the control qubit state.

Signature:

Appearance:

```
CYGate(label=None, ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_51_1.png)

## CZGate

The `CZGate` class creates a controlled-Z gate, applying the **Z** gate according to the control qubit state.

Signature:

Appearance:

```
CZGate(label=None, ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_52_0.png)

## DCXGate

The `DCXGate` class creates a double-CNOT gate. This is a two-qubit gate that has two **CNOT** gates with their control-qubits on different wires.

Signature: Appearance:

![](img/4a3b8ee6681ceda653aea587113057e9_52_1.png)

```
DCXGate()
```

## iSwapGate

The `iSwapGate` class swaps the qubit states of two quantum wires. It also changes the phase of |01⟩ and |10⟩ amplitudes by i.

Signature:

Appearance:

![](img/4a3b8ee6681ceda653aea587113057e9_52_2.png)

```
iSwapGate(label=None, ctrl_state=None)
```

## MCPhaseGate

The `MCPhaseGate` class creates a multi-controlled **Phase** gate with a given number of control qubits.

Signature:

Appearance:

```
MCPhaseGate(lam, num_ctrl_qubits,
label=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_53_0.png)

## MCXGate

The `MCXGate` class creates a multi-controlled **X** gate with a given number of control qubits.

Signature:

Appearance:

```
MCXGate(num_ctrl_qubits=None, label=None,
ctrl_state=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_53_1.png)

## SwapGate

The `SwapGate` swaps the qubit states of two quantum wires.

Signature:

Appearance:

```
SwapGate (label=None)
```

![](img/4a3b8ee6681ceda653aea587113057e9_54_0.png)

## About the Author

**James L. Weaver** is a developer, author, and speaker with a passion for quantum computing. He is a Java Champion, and a JavaOne Rockstar. James has written books including *Inside Java*, *Beginning J2EE*, the *Pro JavaFX* series, and *Java with Raspberry Pi*. As an IBM Quantum Developer Advocate, James speaks internationally about quantum computing with Qiskit at quantum and classical computing conferences.

- 1. 1. Quantum Circuits and Operations
- a. Constructing Quantum Circuits
- i. Using the QuantumCircuit Class
- ii. Using the QuantumRegister Class
- iii. Using the ClassicalRegister Class
- b. Instructions and Gates
- i. The Instruction Class
- ii. The Gate Class
- iii. The ControlledGate Class
- c. Parameterized Quantum Circuits
- i. Creating a Parameter Instance
- ii. Using the ParameterVector Class
- 2. 2. Qiskit Circuit Library Standard Operations
- a. Standard Instructions
- i. Barrier
- ii. Measure
- iii. Reset
- b. Standard Single-Qubit Gates
- i. HGate
- ii. IGate
- iii. PhaseGate
- iv. RXGate
- v. RYGate
- vi. RZGate
- vii. SGate
- viii. SdgGate
- ix. SXGate
- x. SXdgGate
- xi. TGate
- xii. TdgGate
- xiii. UGate
- xiv. XGate
- xv. YGate
- xvi. ZGate
- c. Standard Multi-Qubit Gates
- i. C3XGate
- ii. C3SXGate
- iii. C4XGate
- iv. CCXGate
- v. CHGate
- vi. CPhaseGate
- vii. CRXGate
- viii. CRYGate
- ix. CRZGate
- x. CSwapGate
- xi. CSXGate
- xii. CUGate
- xiii. CXGate
- xiv. CYGate
- xv. CZGate
- xvi. DCXGate
- xvii. iSwapGate
- xviii. MCPhaseGate
- xix. MCXGate
- xx. SwapGate
