17. Note that, depending on the Excel version you are using, you might need to change the `Run64BitRuntime` property of your project to False before running the package, as shown in the following screenshot:

![](bbox=[0.105, 0.187, 0.895, 0.504])

18. Save the package and run it. Observe the data before correction with the Script component and after it. When finished, stop debugging.

# Validating data using regular expressions in a Script component

In this recipe, you will create a Script Component that will use the regular expressions in the `DataProfiling.xml` file to validate the emails of the personal data extracted from a flat file.