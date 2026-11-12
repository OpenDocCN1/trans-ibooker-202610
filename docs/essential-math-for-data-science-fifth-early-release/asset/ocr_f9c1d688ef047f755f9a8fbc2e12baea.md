![](bbox=[0.14, 0.09, 0.86, 0.37])

Figure 6-3. A mix of patients who show symptoms (1) and do not show symptoms (0) exist in the middle.

The way to interpret this is the probability of patients showing symptoms gradually increases with each hour of exposure. Let's visualize this with a **logistic function**, or an S-shaped curve where the output variable is squeezed between 0 and 1, as shown in Figure 6-4.

![](bbox=[0.14, 0.5, 0.86, 0.78])

Figure 6-4. Fitting a logistic function to the data

Because of this overlap of points in the middle, there is no distinct cutoff when patients show symptoms, but rather a gradual transition from 0% probability to 100% probability (“0” and “1”). This example demonstrates how a **logistic regression** results in a curve