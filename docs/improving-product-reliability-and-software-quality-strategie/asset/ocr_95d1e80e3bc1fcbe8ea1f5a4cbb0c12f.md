![](bbox=[0.17, 0.15, 0.78, 0.44])

Figure 7.5 Functional block diagram of a flashlight using Post-its.

everyone is in agreement with the functional block diagram, it is time to begin the fault tree analysis.

The FMEA process can consume significant engineering resources. One way to reduce this time is to prepare the FBD in advance. The top designer or team leader can prepare the FBD prior to the first team meeting. This will save time and speed up the process. It is best to circulate the FBD a week or two before the team’s first meeting for team members to review. If the FBD is developed in advance, it should be reviewed at the first meeting to gain team agreement around which aspects of the design the FMEA will be performed.

# 7.2.2 The Fault Tree Analysis

The FTA is a logical, graphical diagram that describes failure modes and causes. The FTA diagram graphically shows all failures for a system, subsystem, assembly, printed circuit board (PCB), or module. The FTA uses standard logic symbols (Figure 7.6), commonly found in flowcharting for process control, quality control, safety engineering, and so on, to tie together the sequence of events. The output from the FTA provides a better understanding of the causes that can lead to a failure mode. The results of the FTA can then be transferred to an FMEA spreadsheet. The FMEA spreadsheet uses the failure modes and their causes from the FTA and determines the effects of each failure cause on the design. The FMEA spreadsheet is also used to identify the most likely failure modes that will occur, as we will show later. The FMEA spreadsheet sets action plans in place to either reduce or eliminate the possibility of failure.

# 7.2.2.1 Building the Fault Tree

Begin the fault tree by stating the first failure mode from the functional block diagram. The failure mode is defined by taking the first item from the FBD (start with either inputs, processes, or outputs), and turning it into a failure statement. This is usually called the top event or high-level failure mode. If the FBD has as an output of “24 V