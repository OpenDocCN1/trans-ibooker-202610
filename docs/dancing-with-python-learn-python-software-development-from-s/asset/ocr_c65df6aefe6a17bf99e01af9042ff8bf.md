# 9.7.2 Physical and logical qubits

When we run the circuit, we expect to see that the X gate changed the quantum state from |0⟩ to |1⟩. The math tells us that the probability of getting |1⟩ should be 100%, and this is true whether we run the circuit once or a thousand times.

![](bbox=[0.29, 0.21, 0.7, 0.42])

Figure 9.13: Results of circuit execution on a simulator

The histogram in Figure 9.13 shows that we get 1 for every execution of the circuit. We don't display a bar for 0 because it did not occur. The histogram is for a circuit simulated with pseudo-random numbers.

In the histogram in Figure 9.14, I show the result of running the circuit 8,192 times on actual quantum hardware through IBM Quantum on the IBM Cloud. We saw some 0s in addition to the 1s.

![](bbox=[0.29, 0.62, 0.7, 0.83])

Figure 9.14: Results of circuit execution on quantum hardware