![](bbox=[0.22, 0.12, 0.86, 0.26])

As we did before, we run the circuit 1,000 times with the simulator.

```
result = execute(new_circuit, simulator, shots=1000).result()
```

```
counts = result.get_counts(new_circuit)
counts
```

{'01': 1000}

Once again, we get the same result every time, but I must explain the '01'. Qiskit numbers its qubits from top to bottom in drawn circuits and writes out results from right to left. So '01' means that the result of measuring q0 is 1, the right-most value. The measurement from q1 is 0, the next value to the left. The gate and measurement phases of the circuit map |0> to 1 and map |1> to 0.

```
histogram = plot_histogram(counts, color=histogram_color,
    title="IBM Quantum Simulator")
```

<Figure size 504x360 with 1 Axes>

```
histogram
```

![](bbox=[0.28, 0.7, 0.78, 0.9])