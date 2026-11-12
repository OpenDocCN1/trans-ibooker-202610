# Use e to Predict Event Probability Over Time

Let’s look at one more use case for e that you might find useful. Let’s say you are a manufacturer of propane tanks. Obviously you do not want the tank to leak or else that could create hazards, particularly around open flames and sparks. Testing a new tank design, your engineer reports that there is a 5% chance in a given year that it will leak.

You know this is already an unacceptably high number, but you want to know how this probability compounds over time. You now ask yourself “what is the probability of a leak happening within 2 years? 5 years? 10 years?” The more time that is exposed, would not the probability of us seeing the tank leak only get higher? Euler’s number can come to the rescue again!

```
P_{leak} = 1.0 - e^{-\lambda T}
```

The function above models the probability of an event over time, or in this case the tank leaking after T time. e again is Euler’s number, lambda λ is the failure rate across each unit of time (each year), and T is the amount of time gone by (number of years).

If we graph this function where T is our x-axis, the probability of a leak is our y-axis, and λ = .05, here is what we get in Figure A-3: