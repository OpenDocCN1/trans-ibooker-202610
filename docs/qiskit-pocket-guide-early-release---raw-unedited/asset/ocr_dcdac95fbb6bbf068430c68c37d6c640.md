It is sometimes useful to create a quantum circuit in which values may be supplied at runtime. This capability is available in Qiskit using *parameterized circuits*, implemented in part by the `Parameter`, and `ParameterVector` classes.

# Creating a Parameter Instance

The `Parameter` class is used to represent a parameter in a quantum circuit. See “Using the bind_parameters() method” for an example of defining and using a parameterized circuit. As shown in that example, a parameter may be created by supplying a unicode string to its constructor as follows:

```
theta1 = Parameter("θ1")
```

The `Parameter` object reference named `theta1` may subsequently be used in the `bind_parameters()`, or alternatively the `assign_parameters()`, method of the `QuantumCircuit` class.

# Using the ParameterVector Class

The `ParameterVector` class may be leveraged to create and use parameters as a collection instead of individual variables. Example 1-27 contains an example of creating a circuit in which there are three parameterized phase gates. Figure 1-24 shows a drawing of the circuit.

*Example 1-27. Leveraging ParameterVector to create a parameterized circuit*

```
theta = ParameterVector('θ', 3)

qc = QuantumCircuit(3)
qc.h([0,1,2])
qc.p(theta[0],0)
qc.p(theta[1],1)
qc.p(theta[2],2)

qc.draw()
```