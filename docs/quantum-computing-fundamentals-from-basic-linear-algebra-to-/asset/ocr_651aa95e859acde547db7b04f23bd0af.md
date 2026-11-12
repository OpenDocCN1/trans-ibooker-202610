```
.oracle_fc1
# do nothing or I q[0:1]

#.oracle_fc2
# X q[1]

#.oracle_fb3
# CNOT q[0],q[1]

#.oracle_fb4
# CNOT q[0],q[1]
# X q[1]

.measurement
H q[0]
measure q[0]
```

Figure 17.11 shows what this looks like in the editor.

![](bbox=[0.113, 0.463, 0.565, 0.793])

# FIGURE 17.11 Deutsch-Jozsa algorithm

As with the code we saw previously in this chapter, this is primarily just a collection of gates operating on qubits—and that is what a great deal of quantum programming is.

The Deutsch-Jozsa algorithm is relatively easy to implement in QASM. The visualization of the gates and operations shown at the bottom of the editor is quite helpful in understanding quantum algorithms.