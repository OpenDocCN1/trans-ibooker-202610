![](bbox=[0.23, 0.13, 0.785, 0.46])

Figure 3.12 When using up to the 6th degree of polynomial, the model is almost interpolating across the training points. The fit also displays a symptom of high variance in the left region. This is an overfitting case in the classical generalization sense.

The model demonstrates a perfect interpolation across all training points when using up to the 16th degree of polynomial, as shown in figure 3.13. Although the number of features is much larger than the number of training points, the resulting model is not over-sensitive compared to when using up to the 6th degree of polynomial. Adding the roughness penalty brings the model to the over-parameterized regime as the number of polynomial terms increases.