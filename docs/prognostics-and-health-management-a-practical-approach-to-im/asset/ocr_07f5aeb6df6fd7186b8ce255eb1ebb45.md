![](bbox=[0.13, 0.13, 0.77, 0.34])

Figure 2.21 Example of a ringing response from an electrical circuit to an abrupt stimulus.

+   - Sensor cost to isolate, condition, and extract one or more features. Cost includes weight, footprint, power, and reliability of the sensor.
+   - Cost of signal conditioning by the sensor and by the PHM framework.
+   - Prognostic distance (PD): the time between detection of damage leading to failure and the time of functional failure. Functional failure is defined as a level of damage at which an object, such as an SMPS, no longer operates within specifications.
+   - Prognostic resolution and precision of estimates of a future point in time at which functional failure is likely to occur.
+   - Prognostic accuracy (α) and α-distance (PD_α) within which estimates of RUL and SoH must converge with respect to functional end of life (EoL).

# Example 2.13 Suppose the customer specifications for prognostic enabling of the SMPS include the following:

+   - The prognostic system shall detect degradation at least 200 operational days prior to functional failure.
+     - You interpret this as a requirement that the minimum PH must be equal to 200 days. Factors related to this are your ability to design or choose a sensor and data-conditioning algorithms that provide sufficient noise mitigation and selectivity to isolate FD with sufficient lead time before failure.
+     - You also interpret this as being a required resolution in terms of days rather than, for example, weeks or hours.
+     - You obtain customer confirmation.
+   - The prognostic system shall provide estimates of RUL that shall converge to within 5% accuracy at least 72 operational days, ±2 days, before functional failure occurs.
+     - You interpret this as a requirement that sampling must occur at least once every day because of the precision specification of ±2 days.
+     - You also interpret the specifications as a requirement for a minimum PD_α of 72 days with all subsequent estimates within 5% accuracy.
+     - You obtain customer confirmation.