5. The following screen appears. Click on **New Folder** and type `SSISCookBook` as shown in the following screenshot. Click on the **+Add** button to add the folder:

![](bbox=[0.105, 0.168, 0.895, 0.412])

That's it! We're now ready to interact with our local cluster using SSIS.

# Copying data to an on-premises cluster

In this recipe, we'll add a package that will copy local data to the local cluster.

# Getting ready

This recipe assumes that you have access to an on-premises cluster and have created a folder to hold the files in it from the previous recipe.

# How to do it...

+   1. In the solution explorer, open (expand) the `ETL.DW` project and right-click on it to add a new package. Name it `FactOrdersToHDPCluster.dtsx`.