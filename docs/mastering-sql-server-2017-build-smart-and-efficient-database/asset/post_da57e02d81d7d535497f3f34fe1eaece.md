
Microsoft has included JSON as a native data type in SQL Server 2016 and provided a set of functions to accompany the data type.

![](img/07d1225e179d5df9f2b2d26b026386c6_32_0.png)

Further details of JSON can be found in Chapter 3, JSON Support in SQL Server.

## In-Memory OLTP

In-Memory OLTP (codename Hekaton) was introduced in SQL Server 2014. The promise of ultra-high performance data processing inside SQL Server was a major feature when SQL Server 2014 was released. As expected with version-1 features, there were a wide range of limitations in the initial release and this prevented many customers from being able to adopt the technology. With SQL Server 2017, a great number of these limitations have been either raised to a higher threshold or completely removed. In-Memory OLTP has received the required maturity and extension in feature set to make it viable for prime production deployment.

## SQL Server Tools

Accessing or managing data inside SQL Server and developing data solutions are two separate disciplines, each with their own specific focus on SQL Server. As such, Microsoft has created two different tools, each tailored towards the processes and facets of these disciplines.

SQL Server Management Studio (SSMS), as the name suggests, is the main management interface between DBAs/developers and SQL Server. The studio was originally released with SQL Server 2005 as a replacement and consolidation of the old Query Analyzer and Enterprise Manager tools. As with any non-revenue-generating software, SSMS only received minimal attention over the years, with limitations and missing tooling for many of the newer features in SQL Server. With SQL Server 2016, the focus of Microsoft has shifted and SSMS has been de-coupled from the release cycle of SQL Server itself. This decoupling allows both SSMS and SQL Server to be developed without having to wait for each other or for release windows. New releases of SSMS are created on top of more recent versions of Visual Studio and have seen almost monthly update releases since SQL Server 2016 was released into the market.

**SQL Server Data Tools (SSDT)** is also an application based on the Visual Studio framework. SSDT is focused on the application/data development discipline. SSDT is much more closely aligned with Visual Studio in its structure and the features offered. This focus includes the ability to create entire database projects and solution files, easier integration into source control systems, the ability to connect projects into automated build processes, and generally offering a developer-centric development environment with a familiarity with Visual Studio. It is possible to design and create solutions in SSDT for SQL Server using the Relational Engine, Analysis Services, Integration Services, Reporting Services, and of course the Azure SQL database.

Further details of SQL Server Tools can be found in Chapter 2, *SQL Server Tools.*

![](img/07d1225e179d5df9f2b2d26b026386c6_33_0.png)

This concludes the overview of programming enhancements inside SQL Server 2016. The improvements outlined are all solid evolutionary steps in their respective areas. New features are very welcome and allow us to achieve more while requiring less effort on our side. The In-memory OLTP enhancements are especially positive, as they now expand on the groundwork laid down in the release of SQL Server 2014. Please read the respective chapters to gain deeper insight into how these enhancements can help you.

## Business intelligence

Business intelligence is a huge area of IT and has been a cornerstone of the SQL Server product since at least SQL Server 2005. As the market and technologies in the business intelligence space improve, so must SQL Server. The advent of cloud-based data analysis systems as well as the recent buzz around *big data* are driving forces for all data platform providers, and Microsoft is no exception here. While there are multiple enhancements in the business intelligence portion of SQL Server 2016, we will be concentrating on the feature that has a wider audience than just data analysts: the R language in SQL Server.

## R in SQL server

Data analytics has been the hottest topic in IT for the past few years, with new niches being crowned as the pinnacles of information science almost as fast as technology can progress. However, IT does have a few resolute *classics* that have stood the test of time and are still in widespread use. SQL (in its many permutations) is a language we are well aware of in the SQL Server world. Another such language is the succinctly titled **R**. The R language is a data mining, machine learning, and statistical analysis language that has existed since 1993.

Many professionals such as data scientists, data analysts, or statisticians have been using the R language and tools that belong in that domain for a similarly long time. Microsoft has identified that although they may want all of the world's data inside SQL Server, this is just not feasible or sensible. External data sources and languages like R exist and they need to be accessible in an integrated manner.

For this to work, Microsoft made the decision to purchase Revolution Analytics (a commercial entity producing the forked *Revolution R*) in 2015 and was then able to integrate the language and server process into SQL Server 2016. This integration allows a *normal* T-SQL developer to interact with the extremely powerful R service in a native manner and allows more advanced data analysis to be performed on their data.

## Release cycles

Microsoft has made a few major public-facing changes in the past 5 years. These changes include a departure from longer release cycles in their main products and a transition towards subscription-based services (for example, Office 365 and Azure services). The ideas surrounding continuous delivery and agile software development have also shaped the way that Microsoft has been delivering on its flagship integrated development environment Visual Studio, with releases occurring approximately every six months. This change in philosophy is now flowing into the development cycle of SQL Server. Due to the similarly constant release cycle of the cloud version of SQL Server (Azure SQL Database), there is a desire to keep both the cloud and on-premises versions of the product as close to each other as possible. As such, it is unsurprising to see that the previous release cycle of every three to 5 years is being replaced with much shorter intervals. A clear example of this is that SQL Server 2016 released to the market in June of 2016, with a **Community Technology Preview (CTP)** of SQL Server 2017 being released in November of 2016 and the **Release To Market (RTM)** of SQL Server 2017 happening in October 2017. The wave of technology progress stops for no one. This is very clearly true in the case of SQL Server!

## Summary

In this introductory chapter, we saw a brief outline of what will be covered in this book. Each version of SQL Server has hundreds of improvements and enhancements, both through new features and through extensions on previous versions. The outlines for each chapter provide an insight into the main topics covered in this book and allow you to identify which areas you may like to dive into and where to find them.

So let's get going with the rest of the book and see what SQL Server 2017 has to offer.

## 2
## SQL Server Tools

As developers, we are accustomed to using **Integrated Development Environments (IDEs)** in our software projects. Visual Studio has been a major player in the IDE space for many years, if not decades, and it has allowed developers to use the latest software development processes to further improve quality and efficiency in software projects. Server management, on the other hand, has generally been a second-class citizen for many products in the past. In general, this fact can be understood, if not agreed with. IDEs are tools that design and create software that can generate revenue for a business, whereas management tools generally only offer the benefit of some sort of cost-saving, rather than direct revenue generation.

The SQL Server Tools of the past (pre-SQL 2005) were very much focused on fulfilling the requirements of being able to manage and query SQL Server instances and databases but received no great investments in making the tools *comfortable* or even enjoyable to use. Advanced IDEs were firmly in the application development domain and application developers know that databases are a storage system at best and therefore require no elegant tooling to be worked with.

Luckily for us, the advent of SQL Server 2005, along with the release of the .NET Framework, encouraged some people at Microsoft to invest a little more time and resources in providing an improved interface for both developers and DBAs for database and data management purposes. The **SQL Server Management Studio (SSMS)** was born and unified the functionality of two legacy tools: **Query Analyzer** and **Enterprise Manager**. Anyone who has worked with SQL Server since the 2005 release will recognize the application regardless of whether they are using the 2005 release or the latest 2016 build.
