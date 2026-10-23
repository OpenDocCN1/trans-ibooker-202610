If the customer instance you’re working in was created before Q1 2017 release, you may have to explicitly enable Intelligent Services. If this is the case, you have to navigate to Admin Center ▶ Upgrade Center. Search for Recommended Upgrades and locate Intelligent Services and choose Learn More and Upgrade Now. Confirm your changes and Intelligent Services should be enabled.

If the customer instance was created after Q1 2017, Intelligent Services was enabled by default, but you need to ensure that the business rules were imported. This step applies for Employee Central, but it is a good idea to do it, nevertheless. Navigate to Admin Center ▶ Monitor Job and verify that the rules were imported. Figure 12-1 shows a sample screenshot from a prior import. Instances created more recently may not show the import processes, since they were pre-delivered.

![](bbox=[0.11, 0.36, 0.92, 0.51])

Figure 12-1. Checking the Status of Your Business Rule Import

# Standard Event Framework

Most Intelligent Services publishers use the Standard Event Framework, to publish events to SAP and outside of SAP.

Intelligent Services uses existing ODATA APIs to fetch additional information about the event and comprises records of the same type.

Some events only publish to other internal SAP SuccessFactors subscribers, or to external third-party applications, or both.