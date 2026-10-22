
# Set up Porting Assistant for .NET Info

Porting Assistant for .NET > Set up Porting Assistant for .NET

## Porting Assistant for .NET settings

Target framework
Select a target framework to allow Porting Assistant for .NET to assess your solution for .NET Core compatibility.

.NET 6.0.0

AWS named profile
Select an AWS profile to allow Porting Assistant for .NET to assess your solution for .NET Core compatibility. You can also add an AWS named profile using the AWS CLI. Learn more

Add a named profile

Porting Assistant for .NET data usage sharing
When you share your usage data, Porting Assistant for .NET will collect information only about the public NuGet packages, APIs, build errors and stack traces. This information is used to make the Porting Assistant for .NET product better, for example, to improve the package and API replacement recommendations. Porting Assistant for .NET does not collect any identifying information about you. Learn more

I agree to share my usage data with Porting Assistant for .NET - optional
You can change this setting at any time on the Porting Assistant for .NET Settings page.

Cancel Next

Figure 2-16. Running the .NET Porting Assistant from AWS

If we step through the wizard and use the same ASP.NET MVC Web Application we used for the Microsoft Upgrade Assistant we can see that it has correctly identified the solution is targeting .NET Framework 4.7.5, we have seven incompatible NuGet packages and fifteen incompatible APIs. This is the 15 of 17 values highlighted in Figure 2-17 You can see more information about these in the relevant tabs of the Porting Assistant including links to the code that has been identified as incompatible.

## Projects (1) Info

Port your project when you have reached your desired level of compatibility.

Search by project name

|  | Name | Target framework | Referenced projects | Incompatible packages | Incompatible APIs |
|---|---|---|---|---|---|
|  | NetFrameworkMvcWebsite | net472 | 0 | 7 of 14 | 15 of 17 |

Porting Assistant for .NET > Assessed solutions > NetFrameworkMvcWebsite > NetFrameworkMvcWebsite > BundleConfig.cs

## BundleConfig.cs Info

App_Start/BundleConfig.cs

## Code

```
1 using System.Web.Optimization;
2 
3 namespace NetFrameworkMvcWebsite
4 {
5     public class BundleConfig
6     {
7         // For more information on bundling, visit https://go.microsoft.com/fwlink/?LinkId=301862
8         public static void RegisterBundles(BundleCollection bundles)
```

Incompatible method invocation
BundleCollection

NuGet package
Microsoft.AspNet.Web.Optimization 1.1.3

Figure 2-17. AWS Porting Assistant Analysis Results

When you are ready, click Port Solution to begin making the changes to your project files. The application will ask you where you would like to save the ported solution to, having your code in source control means you can choose “Modify source in place”. Unlike Microsoft’s tool, the AWS Porting does actually let you fine tune which versions you would like to upgrade your NuGet packages to in the UI. For this example I have simply left the defaults in place for all packages and stepped through the assistant. You can see now from Figure 2-18 that the project files have been upgraded and the project is now showing in the porting assistant as .NET 6.

![](img/044e6b5616cc4c02f2683d0cc9990365_77_0.png)

Figure 2-18. AWS Porting Assistant Complete

If you click the link to open in Visual Studio you will see the solution loads and your NuGet packages will have been upgraded to versions compatible with .NET Core/6+. Where the Porting Assistant’s help ends however, is by refactoring all the unsupported code. When I try to build this I will still get errors that System.Web.HttpContext does not exist and “the helper directive is not supported” so it may be worth trying out all tools available and comparing your results. Overall the .NET Porting Assistant from AWS does provide a very quick and accessible UI for visualizing and assessing the effort involved in refactoring you .NET Framework code to work with modern .NET.

## Conclusion

The strategies and tools we have covered in this chapter will help you move an existing application running on IIS to AWS. From the basic “Rehosting On AWS” of an application without touching the original source, to “Rearchitecting: Moving to .NET (Core)” and making inroads into modernization of your codebase. Whichever strategy you choose you will benefit from at least some of the advantages of running .NET in the AWS cloud, however, it is worth considering the next steps for your application. Some of the approaches we have covered here will leave your code in the exact same state as it was before you migrated. If you were not actively developing your codebase before the migration then you will not be in a much better position to do so after choosing one of these paths. For a codebase you intend to continue to develop, to iterate on and to add functionality to, then you should plan for a modernization of your legacy application. Modernization will involve not just moving to .NET 6+ (although that will be a prerequisite) it will also involve replacing third party dependencies, replacing external services, and refactoring the architecture of your application to use patterns that better exploit the cloud environment you are now running in. All these will be covered in the next chapter in which I will sell you on the term “serverless” and how it can apply to you as a .NET developer.

- 1 Migrating Applications to the Cloud: Rehost, Refactor, Revise, Rebuild, or Replace?
- https://www.gartner.com/en/documents/1485116/migrating-applications-to-the-cloud-rehost-refactor-revi
- 2 This migration strategy is sometimes called “Refactoring” however, this can be a chameleon of a term so I’ll be sticking to “Rearchitecting” for this book.
- 3 You can find the .NET Framework support policy at https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-framework
- 4 I will admit to looking up from my screen and taking a long sip of coffee before adding “documentation” to this list. My cat is giving me the same knowing stare that you are.
- 5 Your home region in the AWS Migration Hub is the region in which migration data is stored for discovery, planning, and migration tracking. You can set a home region from the Migration Hub Settings page
- 6 Interestingly, the three letter abbreviation MGN for Application Migration Service is a contraction and not an initialism. Perhaps AMS was too similar to AWS.

7 https://docs.aws.amazon.com/app2container/latest/UserGuide/config-containers.html

8 We will revisit these two container orchestration services later in this book, but in a nutshell ECS is a simpler and more “managed” service for running containers without the added complexity of dealing with Kubernetes.

9 https://docs.microsoft.com/en-us/dotnet/core/porting/net-framework-tech-unavailable

# Chapter 3. Modernizing .NET Applications to Serverless

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 4th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at mpotter@oreilly.com.

The term “serverless” can be a source of confusion in software engineering circles. How can you run a web application without servers? Is it really just a marketing phrase? Well in a way, yes it is. Serverless computing is the broad term for backend systems that are architected to use managed services instead of manually configured servers. Serverless applications are “serverless” in the same way that a wireless charger for your smartphone is “wireless”. A wireless charger still has a wire; it comes out of the charger and plugs into the mains outlet, transferring energy from your wall to your desk. The wireless part is only the interface between the charger and your phone. The word “serverless” in Serverless computing is used in the same way. Yes, there are servers, but they are all managed behind the scenes by AWS. The servers are the mains cable plugging your otherwise wireless charger into the wall outlet. The other side, the part visible to us as developers, the part that really matters when deploying and running code,
