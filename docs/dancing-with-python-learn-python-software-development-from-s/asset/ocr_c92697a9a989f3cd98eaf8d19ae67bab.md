```
abs(inverted_amplitudes[0])**2
```

0.7812499999999999

```
inverted_amplitudes[1]
```

0.17677669529663687

```
abs(inverted_amplitudes[1])**2
```

0.03124999999999993

This result is good, but as you can see in the last expression, the probability of getting any other particular ket is still more than 3%.

# Second inversion

Let’s repeat the sign flip and inversion, giving us the left side of Figure 11.10.

![](bbox=[0.16, 0.43, 0.82, 0.65])

Figure 11.10: Second inversion about the mean, followed by a sign flip

The chance of getting |000⟩ is now almost 95%, and the likelihood and getting any other specific ket is less than 1%.

```
amplitudes = inverted_amplitudes
amplitudes[0] = -amplitudes[0]
inverted_amplitudes = invert_about_the_mean(amplitudes)
inverted_amplitudes[0]
```

0.9722718241315027