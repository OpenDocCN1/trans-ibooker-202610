Your team decides to use call polling (with wait for response) for a number of reasons, including the fact that the sensors in the system are *smart sensors*. Upon being polled, they wake up and do the following: (i) sense the node; (ii) perform a series of samples (burst-mode); (iii) convert analog data to digital data; (iv) compute a scalar value of the average of the burst-mode samples; (v) register that scalar value; (vi) inform the calling PHM system (*data ready*); and then (vii) go into quiescence (*sleep*). The advantages of this type of polling include the following: (i) the PHM system determines when data is to be collected and thereby supports the concept of determining the rate of monitoring as specified by node criticality and SoH; (ii) less sensor power is required; (iii) data traffic is reduced; and (iv) a multiplicity of nodes operating at different rates of sampling is supportable.

# 6.4 Initial Design Specifications

Before you can design and develop a prototype PHM system, a set of initial design specifications needs to be prepared, reviewed, and approved. Those specifications include your plans to do the following: (i) use a model-use phase, as illustrated in Figure 6.6; (ii) use results of an initial design and analysis phase (Figures 6.19 and 6.20) to prognostic-enable a number of EMA subsystems (Figure 6.18); and (iii) prognostic enable a system configured as previously shown in Figure 6.23. The system includes the following: (i) a voltage sensor at the output of each SMPS; (ii) three AC current sensors in each EMA; and (iii) positioning commands at the input of each sensor. The requirements for the prototype include the following:

+   1. Prognostic-enable each SMPS for a single mode of failure: output filter, degraded capacitance.
+   2. Prognostic-enable each EMA for a multiplicity of failure modes:
+      a. Power-switch switching transistors, degraded on-resistance.
+      b. Motor transformer, degraded winding.
+      c. Motor shaft, excessive load.
+   3. A single system comprising support for all prognostic-enabled units (as opposed to an individual system for each prognostic-enable unit).
+   4. Checkpoint/restart support for both planned and unplanned unit and system shutdowns.
+   5. Fault management and services are not required in the prototype.

# 6.4.1 Operation: Test/Demonstration vs. Real

To test and/or demonstrate your prototype PHM system, it is impractical to use real devices in real time – too much time is required, even if you use injected faults. You should not, for example, change a value every second to simulate degradation; instead, you should use a larger interval, perhaps five minutes, to allow the test device, component, assembly, and/or subsystem to equilibrate. And you should inject faults that will generate a large number of data points, such as 200 or more. For a five-minute interval, for a single prognostic-enabled mode, your test/demonstration would take more than 15 hours to complete – your customer is not going to wait that long. You need to specify how your team will test and verify the design and operation of a prototype PHM system.