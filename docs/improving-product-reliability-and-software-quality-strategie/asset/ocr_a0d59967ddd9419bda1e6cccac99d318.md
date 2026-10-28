Before starting a software FMEA, the following information is needed:

+   - A functional block diagram of the subsystem to be evaluated, broken down from the subsystem to the module and component level.
+   - Detailed flowchart or sequence diagrams of the software being developed.
+   - A description for each method or function in the FBD or class diagram including the required inputs and expected outputs at the component level.
+   - A description for how the software flowchart and FBD interface and interact.

The software design detail should be down to the lowest level of decomposition that the team will consider. This would include information regarding sequencing, data and control flow, state transition diagrams, messaging, logic description, and communication protocol. The software design detail should include information regarding the interfaces between the software items and the external systems. This would include the hardware-software interface.

# 7.8 Software Fault Tree Analysis (SFTA)

The software fault tree analysis (SFTA), like an FMEA, is a tool to anticipate failures and their underlying causes. The SFTA is an effective tool as part of a quality assurance program for defect prevention. The SFTA can be applied to requirements, design phase, architecture, safety, and to improve the effectiveness of the software verification/validation test plan. The SFTA is not typically applied to low-level code; it is applied at the component/module level and higher. It is an effective and efficient tool to identify weaknesses in the design and stated requirements, safety hazard, and potential failure. A SFTA can identify single points of failure, missing components, ambiguities, uncertainties, and issues with variable values or conditions. If probabilities are assigned to all the low-level causes, SFTA can be used to calculate the probability that a fault will occur. An example of this is shown in Figure 7.12 and based on Table 7.6 for an execution flow failure.

# 7.9 Process FMEAs

The first step in developing a process FMEA is to state desired outcome. What is expected to be achieved or produced from the process that is being evaluated? List the process steps in sequential order that take place in order to produce the desired output. For each process step, list the inputs and outputs that are required for the process. Each process step must have clearly defined requirements for success of the desired output. Assume that each input comes in a good state and should not be the cause for a failure mode. If an input could be the reason for a bad output, then that input should be included as one of the process steps so it can be evaluated for potential failure modes. It is often desired to keep the analysis of the process at a high level of abstraction.

The process FMEA should include all critical processes, including anything that is new or has not been done before. These critical processes can include customer requirements, industrial standards, product and operator safety, product and operator liability, and regulatory and compliance requirements.