Now we run the circuit 1,000 times and see how many 0s and 1s we get. The counts variable holds this data, which is a dictionary of the results we see and the number of times each appears. Each of the 1,000 executions is a shot.

```
result = execute(circuit, simulator, shots=1000).result()
counts = result.get_counts(circuit)
counts
```

{'1': 1000}

We only saw 1s, but let’s display these in a histogram for later comparison.

```
histogram = plot_histogram(counts, color=histogram_color,
    title="IBM Quantum Simulator")
```

<Figure size 504x360 with 1 Axes>

```
histogram
```

![](bbox=[0.28, 0.42, 0.72, 0.66])

Now we use real hardware. While simulators are good for learning, debugging, and small experiments, the future is quantum hardware. Look at it this way: if you have only learned on a flight simulator and not a real airplane, there is no way I will let you fly me from New York to Tokyo. Don’t expect to get a job as a pilot.

Start by loading your IBM Quantum account credentials.

```
IBMQ.load_account()
```

<AccountProvider for IBMQ(hub='ibm-q', group='open', project='main')>