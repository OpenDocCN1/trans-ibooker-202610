+   - The dampening used in the design of the prediction algorithm. A large dampening factor increases the number of FFS data points required to converge to a desired level of accuracy.
+   - The value of the initial-estimated TTF. The larger the initial error, the more FFS data points need to be processed to converge to a desired level of accuracy.
+   - The CBD rate of sampling. The slower the sampling rate, then even if the amount of data processed remains the same, the time between data points is increased, and the longer it takes to reach a desired level of accuracy.

Failure of a prediction algorithm to meet your requirements for convergence efficiency might be caused by many factors other than design, including (i) errors in specifications, such as the initial-estimated value for TTF; (ii) design or operational specifications of the sensor framework; and (iii) insufficient data conditioning that results in overly detrimental offset errors, distortion, and noise in the FFS input.

# 1.7.5 Other Sources of Non-ideality in FFS Data

Other sources of non-ideality in FFS data include, but are not limited to, the following: (i) offset errors in detecting the onset of degradation and the time of functional failure; (ii) distortion in FFS data due to the effects of using noise margins; (iii) measurement uncertainty associated with, for example, quantization errors of data converters; (iv) unfiltered variability caused by environmental effects such as voltage and temperature;

![](bbox=[0.12, 0.52, 0.75, 0.88])

Figure 1.33 Example of FFS data exhibiting an offset error, distortion, and noise.