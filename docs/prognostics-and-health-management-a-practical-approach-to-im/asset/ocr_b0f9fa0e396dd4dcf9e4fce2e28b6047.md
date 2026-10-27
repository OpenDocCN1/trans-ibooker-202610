In this chapter, we presented and discussed sensors attached to nodes to monitor signals, to isolate CBD and FD using noise margins; calibration to use as nominal values for FD; and various transforms and data processing associated with CBD signatures, FFP signatures, DPS data, FFS data, and predications (prediction framework). It makes sense to use a node-based architecture for a framework for a PHM system as opposed to, for example, designing and developing the details for each node in a control- and data-flow framework. A sensor framework of sensors and drivers captures signals at nodes, performs data conditioning, and produces FD. A feature-vector framework of processors performs additional conditioning and transforms to create FFSs used as input data to a prediction framework that produces prognostic information. A health-management framework processes prognostic information to create diagnostic, prognostic, and logistics directives and actions to maintain the health and reliability of the system. A performance-validation framework provides means and methods to produce prognostic-performance metrics for evaluation of accuracy of prognostic information. A control- and data-flow framework manages PHM functions and actions (CAVE3 2015; Hofmeister et al. 2017; Kumar and Pecht 2010; Pecht 2008).

# 3.8 Failure Progression Signatures: Summary

This chapter presented a rationale for transforming CBD signatures into other signatures: FFP, DPS, and FFS. You learned that both an FFP and a DPS can be transformed into an FFS. You saw that an FFP is a normalized version of a CBD and that an FFP greatly reduces the number of signature models; and we have shown that transforming an FFP into an FFS produces a very amenable signature for inputting to prediction algorithms:

+   1. Amplitude values are percentages.
+   2. Amplitudes of 0 or less indicate the absence of detection of degradation.
+   3. Amplitudes of 100 or greater indicate functional failure.

Functional failure is defined as a SoH condition in which a prognostic target is no longer operating within specifications.

You learned that you can transform an FFP into a DPS by solving the signature in terms of a change in a parameter value (*dP*) with respect to a nominal, non-degraded value of the parameter (*P*₀): *DPS* = (*dP/P*₀); you also learned that a DPS is less curvilinear compared to an FFP and that there are two forms of *DPS*: constant and nonconstant degradation rate with respect to time:

*f(DPS, t) = (d/dt)DPS*

You learned that a DPS, like an FFP, can be transformed into an FFS and that a DPS-based FFS is most amenable to processing by prediction algorithms—even when the rate of degradation with respect to time is not constant.

You also learned that you need to verify your modeling choices against the signatures you are modeling and classify whether a signature has a decreasing or increasing slope angle as degradation progresses, to help determine the degradation function for a signature:

*FD*ᵢ = *FD*₀*f(dP*ᵢ, *P*₀)