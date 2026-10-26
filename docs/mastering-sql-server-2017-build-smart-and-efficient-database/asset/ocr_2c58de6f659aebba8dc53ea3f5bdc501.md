8. Now, go to SSMS and open the SSIS Catalog as shown in the following screenshot. You will see that the package has been deployed. We won’t execute it for now as we only wanted to demonstrate how single package deployment works.

![](bbox=[0.305, 0.19, 0.695, 0.32])

# There's more...

The capability to deploy individual packages is possible with SSIS 2016 only. If we have tried to deploy to a SSIS Catalog of a previous version of SSIS (2012-2014), the deployment would have failed. So, to reinforce the point here, SSDT can show you the option that you can deploy individual packages but you’ll need SSIS 2016 to be able to use it.

# Multiple version support

Since its inception, SSIS designer never supported backward compatibility. For example, if you developed a package in SSIS 2014 and tried to deploy it in a SSIS 2012 catalog, you would not be able to do it. Or worse, if you opened a package developed with SSIS 2012 with a SSDT that was used with SSIS 2014, the package was upgraded. When another developer tried to open it with SSDT used for SSIS 2012, he/she was not able to do it. The package was upgraded, period.

With SSIS 2016, SSDT had the following enhancements:

+   - Backward compatibility to prior SSIS versions down to SSIS 2012.
+   - Unified SSDT: as we’ll see later in the book, SSDT can be used for BI components development as well as database development. Prior to SSDT for Visual Studio 2015, it was confusing whether we were using SSDT-BI for BI development or SSDT-SQL for database development.