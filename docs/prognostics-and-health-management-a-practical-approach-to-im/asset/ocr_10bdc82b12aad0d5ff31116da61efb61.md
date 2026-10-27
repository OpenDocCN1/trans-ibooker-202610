# 3

# Failure Progression Signatures

# 3.1 Introduction to Failure Signatures

Chapter 2 introduced three classical prognostic approaches for prognostics and health management/monitoring (PHM): model driven, data driven, and hybrid driven. You were also introduced to usage-based and condition-based approaches. You learned the primary disadvantages of classical and usage-based approaches for prognostics: they are not applicable to a specific prognostic target in a system, and/or they are non-deterministic and not suitable for application to prognostic targets, and/or it is complex to adapt them to sensor data. You also learned that leading indicators of failure can be extracted from sensor data and collected to form condition-based data (CBD) signatures; the modeling and processing of such signatures is a condition-based approach to condition-based maintenance (CBM). Figure 3.1 shows the relationship of an approach using CBD signatures to classical PHM approaches; although the block diagram indicates the approaches are different, a conditioned-based approach often employs analysis and modeling techniques such as reliability modeling, physics of failure (PoF) analysis, and failure mode and effect analysis (FMEA) (Hofmeister et al. 2013, 2016, 2017; Medjaher and Zerhouni 2013; Pecht 2008).

# 3.1.1 Chapter Objectives

The primary objective of this chapter is to take a more in-depth look at CBD signatures and the desirability of transforming those signatures into other signatures that are more amenable as input to prediction algorithms that produce prognostic information. Those other signatures are the fault-to-failure progression (FFP) signature, degradation progression signature (DPS), and functional failure signature (FFS).

A secondary objective is to describe and show how those signatures are used for reliable condition-based monitoring: detecting the onset of degradation, monitoring the increasing progression of damage due to degradation, making prognostic estimates for when damage is likely to reach a level defined as functional failure, and detecting functional failure.

Another objective is to show an important advantage of DPS data and DPS-based FFS compared to CBD signature or FFP signature curves: the former is more linear. A method for quantifying the nonlinearity of FFP-based and DPS-based FFS curves shall be developed and presented in this chapter.