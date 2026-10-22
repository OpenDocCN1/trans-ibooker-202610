![](bbox=[0.38, 0.12, 0.62, 0.25])

Figure 1-24. Example parameterized circuit leveraging ParameterVector

To bind the parameter values to a new circuit, we’ll pass a dictionary that contains the ParameterVector reference and desired list of values to the bind_parameters() method. Example 1-28 shows this technique, and Figure 1-25 shows the bound circuit in which the phase gate parameters are replaced with the supplied values.

# Example 1-28. Binding parameter values to a new circuit

```
b_qc = qc.bind_parameters({theta: [math.pi/8,
    math.pi/4,
    math.pi/2]})

b_qc.draw()
```

![](bbox=[0.38, 0.62, 0.62, 0.75])

Figure 1-25. Example of bound circuit with the supplied phase gate rotation values