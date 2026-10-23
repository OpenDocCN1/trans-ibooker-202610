# D.5.2 Information Required for Modeling the Dispersion of Emissions

+   - Characterize the type of source
+   Point, line, area, volume
+   - Characterize the emissions temperature
+   Vertically directed exit velocity
+   Initial dimension, e.g. stack diameter
+   Emission rate of pollutant of concern
+   - Characterize the immediate area around the source
+   Isolated source/nearby buildings
+   Flat/Significant nearby terrain
+   Land use classification, e.g. agricultural, forest, desert
+   - Characterize the meteorology wind speed and wind direction
+   Vertical temperature gradient, i.e., lapse rate
+   - Existing (background) ambient concentrations of the pollutant of concern
+   - Develop a plan for predicting the ground-level concentrations produced by the source, often called a modeling protocol
+   Obtain protocol approval from the appropriate entities

The currently accepted steady-state dispersion model for evaluating the ground-level concentrations in the United States is AERMOD. The following is taken from the AERMOD Model Formulation and Evaluation report, EPA-454/ R-17-001, May 2017.

In the following AERMIC stands for the AMS/EPA Regulatory Model Improvement Committee comprised of American Meteorological Society (AMS) and US Environmental Protection Agency (EPA) scientists.

The AERMOD modeling system consists of two pre-processors and the dispersion model. The AERMIC meteorological preprocessor (AERMET) provides AERMOD with the meteorological information it needs to characterize the PBL. The AERMIC terrain pre-processor (AERMAP) both characterizes the terrain, and generates receptor grids for the dispersion model (AERMOD).

AERMET uses meteorological data and surface characteristics to calculate boundary layer parameters (e.g. mixing height, friction velocity, etc.) needed by AERMOD. This data, whether measured off-site or on-site, must be representative of the meteorology in the modeling domain. AERMAP uses gridded terrain data for the modeling area to calculate a representative terrain-influence height associated with each receptor location. The gridded data is supplied to AERMAP in the format of the Digital Elevation Model (DEM) data. The terrain preprocessor can also be used to compute elevations for both discrete receptors and receptor grids.

AERMOD is a steady-state plume model. In the stable boundary layer (SBL), it assumes the concentration distribution to be Gaussian in both the vertical and horizontal. In the convective boundary layer (CBL), the horizontal distribution is also assumed to be Gaussian, but the vertical distribution is described with a bi-Gaussian probability density function (pdf). Additionally, in the CBL, AERMOD treats “plume loft- ing,” whereby a portion of plume mass, released from a buoyant source, rises to and remains near the top of the boundary layer before becoming mixed into the CBL. AERMOD also tracks any plume mass that penetrates into the elevated stable layer, and then allows it to re-enter the boundary layer when and if appropriate. For sources in both the CBL and the SBL AERMOD treats the enhancement of lateral dispersion resulting from plume meander.

Using a relatively simple approach, AERMOD incor- porates current concepts about flow and dispersion in complex terrain. Where appropriate, the plume is mod- eled as either impacting and/or following the terrain. This approach has been designed to be physically realis- tic and simple to implement while avoiding the need to distinguish among simple, intermediate and complex terrain, as required by other regulatory models. As a result, AERMOD removes the need for defining complex terrain regimes.

# D.5.3 Screening Modeling

EPA’s currently accepted screening dispersion model is AERSCREEN. In keeping with the screening approach of the past four decades, AERSCREEN calculates the expected maximum ground-level concentration for multiple averaging periods (1-hour to annual) for a single emission point. It uses the primary components of the AERMOD modeling system but uses an additional program, MAKEMET, to create the meteorological conditions that have been shown to produce the maximum ground-level concentration from the source and land classification/land use characteristics. A quick round of screening modeling is often all that is required to demonstrate compliance with air quality standards.

The MAKEMET program has a long complex history, dating back over 10 years. Suffixe it to say that the version of MAKEMET supplied with AERSCREEN operates in the following manner. As mentioned earlier, MAKEMET gener- ates a matrix of meteorological conditions for applica- tion of AERMOD in a screening mode and output the results in the form of AERMOD-ready surface and profile meteorological data files. The matrix is generated based on looping through a range of wind speeds, cloud covers, ambient temperatures, solar elevation angles, and convec- tive velocity scales (w* for convective conditions only) for