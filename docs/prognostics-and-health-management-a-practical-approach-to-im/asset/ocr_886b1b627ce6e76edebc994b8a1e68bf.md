![](bbox=[0.114, 0.106, 0.61, 0.69])

**Figure 5.11** Example plots: ideal input and non-ideal output from an ADC.

In addition to quantization errors, offset errors, step errors, gain errors, and nonlinearity errors, other errors can be injected into digital data. Causes of those errors are many and include the following: clock jitter, aperture errors, voltage droop during the hold period of sample-and-hold input stages, thermal noise because of large capacitance values used in sample-and-hold input stages, and nonlinear gain in the various stages used in data converters.

**Example 5.7** Suppose the ADC from the previous example is used to measure the current flowing through an RTD sensor. Also suppose a second instantiation of that ADC is used to measure the voltage applied across the same RTD, and both measurements