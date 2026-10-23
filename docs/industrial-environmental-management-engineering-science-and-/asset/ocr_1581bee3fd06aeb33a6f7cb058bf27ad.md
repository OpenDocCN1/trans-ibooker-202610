user-specified surface characteristics (Z₀, B₀, r). For stable cases, the mechanical mixing height (Zₘ) is calculated based on the friction velocity, u*. A loop through Zₘ factors (multiplied times the initial value calculated form u*) is also included to account for smoothing of Zₘ that occurs with refined AERMET data. Stable transition cases with solar angle greater than zero but less than the critical solar angle (ACRIT) are also included in the matrix.

AERSCREEN is an interactive wrapper for the AERMOD modeling system and computes input values according to user supplied values in response to prompts printed to the screen.

Before running AERSCREEN, users should consult and become familiar with the following:

+   AERMOD User’s Guide (USEPA 2016a)
+   AERMAP User’s Guide (USEPA 2016b)
+   AERMET User’s Guide (for surface characteristics tables) (USEPA 2016c)
+   BPIPPRM User’s Guide (USEPA 2004) – relates to building effects on dispersion
+   AERSURFACE User’s Guide (USEPA 2008)
+   AERMOD Implementation Guide (USEPA 2016e)

Although the next two references are specific to the United States, there is useful information that may be important to dispersion modeling in general.

Guideline on Air Quality Models (Appendix W) (USEPA 2016d)

Screening Procedures for Estimating the Air Quality Impact of Stationary Sources (USEPA 1992)

A link to AERSCREEN (Version 16216) code including the latest MAKEMET code (Version 16216) is provided below along with documentation and test cases. The MAKEMET program interfaces with AERSCREEN to generate a site-specific matrix of screening meteorological conditions based on user inputs for input into AERMOD.

AERSCREEN code and executables (v16216) (ZIP)(759 K)
MAKEMET code and executables (v16216) (ZIP)(355 K)

Please note that before using AERSCREEN, users must have the latest AERMOD executable (18081) as well as the AERMAP terrain preprocessor and BPIPPRM executables. These programs can be downloaded from the AERMOD Modeling System section.

# D.5.4 AERSCREEN Examples

Figure D.4 was produced in response for an analysis of emissions from an anaerobic digester used to produce methane for power generation. Analysis showed that the methane also contained sulfur compounds. It provides an example of the effect of stack height on ground level

Screening concentrations with changing stack heights
(10 ton per year SO2 emissions)

![](bbox=[0.115, 0.59, 0.61, 0.9])

Figure D.4 SO₂ concentration with changing heights.