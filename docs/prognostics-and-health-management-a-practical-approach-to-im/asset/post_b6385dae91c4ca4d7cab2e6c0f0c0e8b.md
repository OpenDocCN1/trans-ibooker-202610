
![](img/05efaadbca64ad00f9e8d44c1db8cc7b_298_0.png)

**Figure 6.44** Phase A currents: transistor fault in the positive Phase A portion.

## H-Bridge Controller: FFP and FFS Signatures

You obtain data with and without injected on-resistance faults in an H bridge controller. To do that, you cause the on-resistance of the power-switching transistor to increase over time in the positive half of the Phase A section of the H bridge. You plot the special peak RMS values for Phase A, Phase B, and Phase C as shown in Figures 6.44–6.46.

### FFP – PHASE B - FAULT IN PHASE A

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_299_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_299_1.png)

Figure 6.45 Phase B currents: transistor fault in the positive Phase A portion.

The results are not what you expected: rather than a decrease only in the positive half of the Phase A current, there is a change in both the positive and negative halves of all three phase currents; also see Figure 6.47. Before proceeding, you need to understand what is happening. Examination of the motor circuit (Figure 6.48) leads you to conclude that the common node for the three motor windings is floating, which means a change in amplitude of one phase current will change the reference level for all three phase currents.

### FFP – PHASE C - FAULT IN PHASE A

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_300_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_300_1.png)

Figure 6.46 Phase C currents: transistor fault in the positive Phase A portion.

You then conclude that you can use the results as follows:

- 1. Sum the absolute value of each half of all three phase currents.
- 2. Use the highest-valued sum as an FFP data point.
- 3. The location of the fault is in the section of the H bridge corresponding to the largest decrease in magnitude. In this case, that is the positive half of the H bridge for the Phase A current (refer back to the top plot of Figure 6.44).
- 4. When you do, the resulting FFP is shown in Figure 6.49.

### FFP - PHASE A TRANSISTOR FAULT STARTS @ 700

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_301_0.png)

Figure 6.47 Peak-RMS currents: transistor fault in the positive Phase A portion.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_301_1.png)

Figure 6.48 Close-up of the EMA motor winding.

After examining Figure 6.49 and reviewing your experiment and experimental results, you conclude the following:

- 1. You have a prognostic solution to diagnose and prognose a failing power-switching transistor in an H bridge driver in an EMA.
- 2. You use the following to convert feature data to FFP signature data:

```
FFP = (ABS(P_PK) + ABS(N_PK) + 240)/120 sum absolute peak values    (6.8)
```

where 120 = nominal current value in the absence of degradation.

### FFP - PHASE A TRANSISTOR FAULT STARTS @ 700

Figure 6.49 FFP: H bridge fault, sum of both halves of the Phase A current.

- 3. You use a four-point averaging method to smooth the FFP signature data (see Figure 6.50).
- 4. You can use Eqs. (6.6) and (6.7) to transform FFP signature data to FFS data (see Figure 6.51).
- In Eq. (6.6), change 0.025 to 0.10 for an NM as a further step toward noise mitigation.

### SMOOTHED FFP - PHASE A TRANSISTOR FAULT STARTS @ 700

Figure 6.50 Smoothed FFP: H bridge fault.

### FFS - PHASE A TRANSISTOR FAULT STARTS @ 700

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_303_0.png)

Figure 6.51 Smoothed FFS: H bridge fault.

- - In Eq. (6.7), continue to use 0.25 for functional failure (about a 25% reduction in current).
- - Apply five-point averaging to the FFS data as a final step toward noise mitigation.

During a design review, a reviewer questions your use of Eq. (6.8) rather than the following (from Chapter 3):

```
FFP_i = (FD_i - FD_0)/FD_0
(3.8)
```

You explain that when the nominal value of a feature, FD_0, is less than 1, and especially when the functional-failure threshold is less than 1, it is more appropriate to use an alternative model:

```
FFP_i = (FD_i - FD_0)
(6.9)
```

## 6.6 Diagnostic and Prognostic Procedure

The specifications include a description of the diagnostic and prognostic procedures for an SMPS power supply and an EMA.

### 6.6.1 SMPS Power Supply

The diagnostic procedure for the power supply is straightforward: transform sensor data into an FFP signature, then into DPS data, and then into FFS data; and then input the FFS data to a prediction framework. When SoH is less than 100 and greater than 0, the power supply is degraded. When SoH first falls below 100, a diagnosis can be made. Issue alerts as specified in the node definition.

### 6.6.2 EMA

The sensors in the sensor framework produce three sets of measurements, one for each of the three phase currents – both positive and negative scalar values. Sensor measurement data is processed by computational routines (software and/or firmware) to produce three pairs of peak-RMS values: one pair of positive and negative values for each phase.

**Fault-Tree Processing: Detection and Diagnosis**

Check the amplitudes of the positive and negative special-peak RMS values for each phase current:

- 1. When both values for a phase exhibit decreasing amplitudes:
  - Fault mode: increasing on-resistance in the switching power transistors for that phase.
  - Fault isolation: the transistor in the H bridge circuit for the phase half that exhibits the largest decrease in amplitude.
- 2. When both values for all three phase currents exhibit increasing amplitudes:
  - Fault mode: increasing load on the EMA motor.
  - Fault isolation: either (or both) increasing friction or weight exerted on the EMA motor.
- 3. When both values for a single (or two) phase currents exhibit increasing amplitudes:
  - Fault mode: reduced impedance, such as that from shorting between turns in a motor winding.
  - Fault isolation: motor winding for the phase exhibiting increased current.
- 4. Otherwise, EMA status equals *no fault found*.

## 6.7 Specifications: Robustness and Capability

Your customer approves your initial design specifications and approves further design and development of a prototype PHM system to demonstrate robustness and capability by concurrently monitoring, detecting levels of degradation, and issuing appropriate alerts and prediction information for the system configuration shown in Figure 6.18: (i) a nondegraded power supply, (ii) a degraded power supply, (iii) a nondegraded EMA, (iv) an EMA with a degraded power MOSFET, and (v) an EMA with excessive loading on the motor shaft. Data saved in the output file labeled DS2 – |7| in Figure 6.29 – and the output file labeled DS4 – |24| in Figure 6.30 – provide input data to test, verify, and demonstrate the system.

A set of final specifications for the robust prototype of an exemplary PHM system includes the following: (i) special root mean square (RMS) method for processing phase currents; (ii) prognostic-enabling of the power supplies; and the EMAs for power-switching MOSFETs and motor loading. The final specifications need to support your claim that your team will develop a robust prototype of an exemplary PHM system that is capable of providing the required prognostics.

### 6.7.1 Node-Based Architecture

Your PHM system needs to define the polling schema for each node, to include scope, definition, and interval. Scope includes, for example, which program(s) is (are) to be used to process the collected data and, of course, requirements related to data conditioning and predictions. Those and other considerations lead you to design an online, node-based architecture such as the annotated diagram in Figure 6.52, which includes your PHM system and others. You are not designing, for example, a system to perform CBM, a system to perform autonomous logistic support, or a system to support and provide fault management.

Your PHM system is to include the following major functions and capabilities:

- - System control and data flow.
- - A system-node definition, and node definitions for two EMA subsystems, each comprising a power supply and two EMA assemblies.
- - A library of data sets: input, output, and checkpoint/restart data sets to support interruptible PHM operations.
- - Computational libraries containing program modules and routines you provide to perform data conditioning, fusing, transforming, and anything else required to support the PHM system.
- - A library of system services to support and provide, for example, graphical and end-user interfaces, messages and alerts, information display and printing, and event handlers.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_305_0.png)

**Figure 6.52** Block diagram for an example of a robust PHM system.
