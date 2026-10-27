# SENSING SYSTEM
OFFLINE PHASE: Learning, Evaluation, and Development

Monitor Data

Extract Features: Condition Indicators

Select Features: Characterize, Evaluate, Reduce, & Select

Create Models & Methods

Library: Models Methods

Data Quality  Data Transformation, Conditioning  Data Modeling and Computational Methods

{CBD}i  {FD}i  {V_Ri}  {FFP}i {DPS}i (FL_FFP) (FL_DPS) {FFS}i

Figure 6.14 Offline phase to develop a prognostic-enabling solution for a PHM system.

# SENSING SYSTEM
ONLINE PHASE:
Data Acquisition and Manipulation

Library: Models & Methods

Monitor Data

Extract Features: Condition Indicators

Use Models & Methods

{CBD}i  {FD}i = V_R,i  {FFP}i {DPS}i (FL_FFP) (FL_DPS) {FFS}i

Data Acquisition  Data Manipulation: Transformation and Conditioning

Figure 6.15 Online phase to exploit a prognostic-enabling solution.

# 6.1.6 Chapter Objectives

The methods presented in the previous chapters provide a solid foundation for designing and developing individual prognostic solutions. This chapter illustrates important aspects and considerations in the design and development of a robust prototype of an exemplary PHM system; this information is presented as though you are the lead designer of such a PHM system.

Although single-node PHM solutions are useful in investigating, characterizing, developing, and demonstrating single- or limited-sensing system solutions of simple, limited capability, they are impractical in a real, complex system with multiple nodes, different types of sensors, single and multivariate signal processing, and varying sample rates at different sampling frequencies. And for prognostics, system solutions need a robust architecture of frameworks, control and data flow, and memory, to do the following: (i) sufficiently remember the past – what has previously transpired at each node; (ii) sense the present – collect measurement data from each node; (iii) process the present and combine it with the past to project the future – produce and act on prediction information for each node. A very important consideration is that a PHM system is not going to run forever: it will be intentionally and unintentionally paused