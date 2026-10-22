
| Names | Example | Notes |
|---|---|---|
| iSwap | `qc.iswap(0,1)` | Swaps the qubit states of wires 0 & 1, and changes the phase of the |01⟩ and |10⟩ amplitudes by i. See "iSwapGate". |
| MCP, Multi-control Phase | `qc.mcp(math.pi/4, [0,1,2],3)` | Applies the Phase gate to quantum wire 3, subject to the state of the control qubits on wires 0, 1 and 2. See "MCPhaseGate". |
| MCX, Multi-control X | `qc.mcx([0,1,2],3)` | Applies the X gate to quantum wire 3, subject to the state of the control qubits on wires 0, 1 and 2. See "MCXGate". |
| Swap | `qc.swap(2,3)` | Swaps the qubit states of wires 2 & 3. See "SwapGate". |

Figure 1-2 contains a nonsensical circuit with all of the multi-qubit gate examples from Table 1-2

![](img/4a3b8ee6681ceda653aea587113057e9_10_0.png)

## Drawing a Quantum Circuit

The draw() method draws a quantum circuit in various formats.

*Using the draw() method*

Example 1-1 contains an example that uses the draw() method in the default format.

### Example 1-1. Example of using the draw() method

```
qc = QuantumCircuit(3)
qc.h(0)
qc.cx(0, 1)
qc.cx(0, 2)
qc.draw()
```

Figure 1-3 shows the drawn circuit.

![](img/4a3b8ee6681ceda653aea587113057e9_11_0.png)

## Creating a Barrier

The barrier() method places a barrier on a circuit (shown in Figure 1-4), providing both visual and functional separation between gates on a quantum circuit. Gates on either side of a barrier are not candidates for being optimized together as the circuit is converted to run on quantum hardware or a simulator.

### NOTE

The set of gates expressed using Qiskit represents an abstraction for the actual gates implemented on a given quantum computer or simulator. Qiskit transpiles the gates into those implemented on the target platform, combining gates where possible to optimize the circuit.

### Using the barrier() method

The barrier() method takes as an optional argument the qubit wires on which to place a barrier. If no argument is supplied, a barrier is placed across all of the quantum wires. This method creates a Barrier instance (see “Barrier”).

Example 1-2 demonstrates using the barrier() method with and without arguments.

### Example 1-2. Example of using the barrier() method

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

![](img/4a3b8ee6681ceda653aea587113057e9_12_0.png)

Notice that the **S** gates in the circuit are separated by a barrier, and therefore not candidates to be combined into a **Z** gate. However the **X** gates may be combined by removing both of them, as they cancel each other.

## Measuring a quantum circuit

The methods commonly used to measure quantum circuits are `measure()` and `measure_all()`. The former is useful when the quantum circuit contains classical wires on which to receive the result of a measurement. The latter is useful when the quantum circuit doesn’t have any classical wires. These methods create `Measure` instances (see “Measure”).

### *Using the measure() method*

The `measure()` method takes two arguments:

- the qubit wires to be measured
- the classical wires on which to store the resulting bits

Here is an example of using the `measure()` method, as well as a drawing of the resultant circuit:

### Example 1-3. Example of using the measure() method

```
qc = QuantumCircuit(3, 3)
qc.h([0,1,2])
qc.measure([0,1,2], [0,1,2])
qc.draw()
```

![](img/4a3b8ee6681ceda653aea587113057e9_14_0.png)

Figure 1-5. Example circuit using the measure() method

Notice that the `measure()` method appended the requested measurement operations to the circuit.

### Using the measure_all() method

The `measure_all()` method may be called with no arguments. Here is an example of using the `measure_all()` method, as well as a drawing of the resultant circuit:

### Example 1-4. Example of using the measure_all() method

```
qc = QuantumCircuit(3)
qc.h([0,1,2])
qc.measure_all()
qc.draw()
```

![](img/4a3b8ee6681ceda653aea587113057e9_15_0.png)

Figure 1-6. Example circuit using the measure_all() method

Notice that the `measure_all()` method created three classical wires, and added a barrier to the circuit, before appending the measurement operations.

## Obtaining information about a quantum circuit

Methods commonly used to obtain information about a quantum circuit include `depth()`, `size()`, and `width()`. These are listed in Table 1-3. Note that variable `qc` refers to an instance of QuantumCircuit.

Table 1-3. Methods commonly used to obtain information about a quantum circuit

| Names | Example | Notes |
|---|---|---|
| `depth` | `qc.depth()` | Returns the depth (critical path) of a circuit if directives such as barrier were removed |
| `size` | `qc.size()` | Returns the total number of gate operations in a circuit |
| `width` | `qc.width()` | Return the sum of qubits wires and classical wires in a circuit |

Attributes commonly used to obtain information about a quantum circuit include `clbits`, `data`, `global_phase`, `num_clbits`, `num_qubits`, and `qubits`. These are listed in Table 1-4. Note that variable `qc` refers to an instance of `QuantumCircuit`.

### Table 1-4. Attributes commonly used to obtain information about a quantum circuit

| Names | Example | Notes |
|---|---|---|
| `clbits` | `qc.clbits` | Obtains the list of classical bits in the order that the registers were added |
| `data` | `qc.data` | Obtains a list of the operations (e.g. gates, barriers, and measurement operations) in the circuit |
| `global_phase` | `qc.global_phase` | Obtains the global phase of the circuit in radians |
| `num_clbits` | `qc.num_clbits` | Obtains the number of classical wires in the circuit |
| `num_qubits` | `qc.num_qubits` | Obtains the number of quantum wires in the circuit |
| `qubits` | `qc.qubits` | Obtains the list of quantum bits in the order that the registers were added |

## Manipulating a quantum circuit

Methods commonly used to manipulate quantum circuits include `append()`, `bind_parameters()`, `compose()`, `copy()`, `decompose()`, `from_qasm_file()`, `from_qasm_str()`, `initialize()`, `reset()`, `qasm()`, `to_gate()`, and `to_instruction()`.

### *Using the append() method*

The `append()` method appends an instruction or gate to the end of the circuit on specified wires, modifying the circuit in place.

Example 1-5 contains an example of using the `append()` method, and Figure 1-7 shows a drawing of the resultant circuit.

### Example 1-5. Example of using the append() method

```
qc = QuantumCircuit(2)
qc.h(1)
cx_gate = CXGate()
qc.append(cx_gate, [1,0])
qc.draw()
```

![](img/4a3b8ee6681ceda653aea587113057e9_17_0.png)

Figure 1-7. Example circuit resulting from the append() method

NOTE
The CXGate class (see “CXGate”) used in the example is one of the gates defined in the qiskit.circuit.library package.

### Using the bind_parameters() method

The bind_parameters() method binds parameters (see “Creating a Parameter Instance”) to a quantum circuit. Example 1-6 contains an example of creating a circuit in which there are three parameterized phase gates. Figure 1-8 shows a drawing of the circuit.

### Example 1-6. Creating a parameterized circuit

```
theta1 = Parameter('θ1')
theta2 = Parameter('θ2')
theta3 = Parameter('θ3')

qc = QuantumCircuit(3)
qc.h([0,1,2])
qc.p(theta1,0)
qc.p(theta2,1)
qc.p(theta3,2)

qc.draw()
```

![](img/4a3b8ee6681ceda653aea587113057e9_18_0.png)

Figure 1-8. Example parameterized circuit

To bind the parameter values to a new circuit, we’ll pass a dictionary that contains the parameter references and desired values to the bind_parameters() method. Example 1-7 shows this technique, and Figure 1-9 shows the bound circuit in which the phase gate parameters are replaced with the supplied values.

### Example 1-7. Binding parameter values to a new circuit

```
b_qc = qc.bind_parameters({theta1: math.pi/8,
    theta2: math.pi/4,
    theta3: math.pi/2})
```

```
b_qc.draw()
```
