In this chapter you will learn how to choose, plan and execute the migration of a web application running on IIS and built on either .NET Framework or .NET Core/6+

> **NOTE**
> 
> With the release of .NET 5 in November 2020 Microsoft has renamed .NET Core to simply “.NET”. In these next chapters we will refer to .NET Core and all future versions as .NET and the previous, legacy version of the framework as .NET Framework

# Choosing a Migration Path

Every .NET application will have a different path to the cloud and, while we cannot create a one-size-fits-all framework for migrating a legacy .NET application, we can learn from the migrations of those that came before us. In 2011, the technology research company Gartner identified 5 migration strategies for migrating on premises software to the cloud¹. These were known as The 5 R’s and over the years have been refined, adapted and expanded as new experiences emerged, growing to encompass all the challenges you might face migrating and modernizing a legacy web application.

For migrating some of your code to AWS we now have 6 R’s, any of which could be applied to a legacy .NET web application running on IIS and built in either .NET Framework or the more recent incarnation .NET.

+   - Rehosting
+   - Replatforming
+   - Repurchasing
+   - Rearchitecting
+   - Rebuilding
+   - Retaining