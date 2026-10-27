![](bbox=[0.08, 0.12, 0.78, 0.35])

Figure 3.32 Simulated CBD plots using nonlinear and linear degradation rates.

on the right side of Figure 3.32, when the nonlinearity in the degradation rate is increased even further: for example, to dP = (0.75/200)t^2.0.

The DNL term introduced in 3.18 and a related term, integral nonlinearity (INL), are measurements of the deviation between two analog values: in this book, the two analog values are an ideal (straight line) signature and the signature of the data, such as an FFS, that is input to prediction algorithms (Texas Instruments 1995). In this book, we introduce a new term, FFS nonlinearity (FNL), which is a measurement of the deviation between ideal an RUL and an ideal SoH transfer curve and actual curves of the estimates of RUL and SoH from such prediction algorithms (Hofmeister et al. 2018).

# Example 3.19

Transform the simulation data you created for the right-hand plot shown in Figure 3.32 to DPS data, assuming a constant degradation rate. When you do so, you create a nonlinear DPS, a transfer curve, as shown in Figure 3.33; an ideal DPS is also shown for comparison. Even though the DPS is nonlinear, it is more linear than its FFP signature data (refer back to the right-hand plot in Figure 3.32).

# 3.7 Failure Progression Signatures and System Nodes

In Chapter 1, we used an example illustration of a framework for a PHM system for CBM. Referring to Figure 3.34, the framework comprises (i) a sensor framework, (ii) a feature-vector framework, (iii) a prediction framework, (iv) a health-management framework, (v) a performance-validation framework, and (vi) a control- and data-flow framework. The sensor framework is attached to a node, and in a system there might be hundreds or thousands of nodes, each of which is a point in a system that is a source of CBD: acoustic, electrical, magnetic, mechanical stresses and strains, vibration-based, radiation, optical, and so on. It makes sense, then, to use a node-based architecture as an anchor for a PHM system that collects CBD for reliable condition monitoring to support CBM.