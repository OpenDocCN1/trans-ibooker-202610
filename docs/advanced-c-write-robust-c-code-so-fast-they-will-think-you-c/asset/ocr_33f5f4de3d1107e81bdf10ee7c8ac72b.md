+   7. Click on the **Run** button to recompile the code – now, we get the following error:

![](bbox=[0.07, 0.11, 0.87, 0.3])

Figure 3.46: Failed conversion of int to Voltage&

The same line has the problem that we ran previously, but for a different reason. So, *implicit conversion only works with value types.*

+   8. Comment out the line generating the error, and then, after the call to **use_float(42)**, add the following line:
+   
+       use_float(volts);

+   9. Click on the **Run** button to recompile the code – now we get the following error:

![](bbox=[0.07, 0.51, 0.87, 0.68])

Figure 3.47: Failed conversion of Voltage to float