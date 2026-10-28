make sure that the liquid used will not boil within the temperature range to which the product will be stressed. You can work with your coolant supplier to select a higher temperature rated liquid for this application. We use HFE 7500, which boils at 128 °C.

# 8.1.4 Planning for HALT Testing

Before the product can go through HALT, some planning is in order. There can be a significant amount of preparation work required before HALT testing. The reliability engineer and the lead engineer should work together to have everything in place for the day the HALT process is to begin. The following is a list of items that should be ready for the test:

+   1. The product, hopefully, three to five working units, and a spare. The spare is often called a golden unit because it is not intended for stress testing; it is used when there are subtle testing issues and it is difficult to tell if the DUT or the test instrumentation is at fault. Inserting the golden unit will verify if the problem exists with the DUT. This can speed the troubleshooting process greatly. Thus, the information learned by using this unit is “golden.”
+   2. Test instrumentation. This is probably the most important item on the list after the product itself because the failures have to be discovered and corrected. Poor monitoring will miss some failures and render the HALT process less effective than it might have been.
+   3. The output specifications that will be monitored and the monitoring instrumentation.
+   4. Documentation, i.e. schematics, assembly drawings, flowcharts, and so on.
+   5. A mechanical fixture to affix the DUT to the HALT table.
+   6. Input and output cabling.
+   7. Special devices, i.e. liquid cooling apparatus, air ducts, power sources, other support devices, and so on.
+   8. Software, where required.
+   9. The stress levels intended to be applied to the DUT (established and agreed to by the HALT team). What are the stress limits that can be applied to the product? If the product has built-in protection circuitry for temperature, can it be overridden to allow for higher thermal stress? The HALT test plan should include verification that the temperature protection circuitry works before enabling a mitigation plan to test beyond those monitor limits. The agreed upon test limits should be documented in a table such as that shown in Table 8.1.
+   10. The time required for the testing.
+   11. The lead engineer needs to be available for the entire HALT process.
+   12. A test engineer assists in failure analysis.
+   13. The reliability engineer and a HALT chamber operator – the reliability engineer usually writes the final HALT test report.

The HALT team starts by placing the DUT in the HALT chamber and interconnecting it to the power sources, loads, and instrumentation. Then, the DUT is turned on and monitored to verify if it is operating properly. This step is needed to ensure the test setup is functioning properly. The runtime here is determined by how long it will take