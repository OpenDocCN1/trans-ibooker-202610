# Test Setup Verification

![](bbox=[0.24, 0.13, 0.77, 0.3])

Figure 8.10 Test setup profile to checkout connections and functionality.

# Temperature Step Stress & Power Turn On

![](bbox=[0.2, 0.37, 0.77, 0.58])

Figure 8.11 Temperature step stress with power cycle and end of each step.

If testing ends at a cold temperature (i.e. a hard failure at −20 °C), the DUT should be brought back to room temperature prior to opening the chamber doors. The chamber may get to room temperature quickly, but it is important to wait for the product to reach room temperature as well. This will prevent condensation from forming on the product if the chamber is open when the product is still at a cold temperature.

Temperature testing begins by stepping down to lower temperatures in 10–20 °C increments with dwell times of typically 10 minutes. The idea is to start with the weakest stress and move to stronger stresses as the stress testing continues. This way, subtle failures will not be lost with excessive stress testing. The time at dwell is driven by the time it takes the product to reach 90% of the target temperature and the test instrumentation diagnostics/checkers run time required to complete a full test.

Placing a thermal couple on the product under test in an area where there is high thermal mass will allow you to monitor the product temperature and ensure that the products reaches temperature before checkers/diagnostics start running. Most HALT chambers provide an option to control the chamber air based on the air temperature or the product temperature. Selecting product temperature is preferred when the product