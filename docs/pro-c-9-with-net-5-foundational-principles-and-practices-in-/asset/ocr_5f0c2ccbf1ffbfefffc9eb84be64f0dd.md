# Simplifying COM Interoperability Using Dynamic Data (Windows Only)

Let’s see another useful case for the dynamic keyword within the context of a COM interoperability project. Now, if you do not have much background in COM development, do be aware for this next example that a compiled COM library contains metadata, just like a .NET Core library; however, the format is completely different. Because of this, if a .NET Core program needs to communicate with a COM object, the first order of business is to generate what is known as an *interop assembly* (described in the following paragraphs). Doing so is quite straightforward.

> **Note** If you did not install the Visual Studio Tools for Office (VSTO) individual component or “Office/SharePoint development” workload, you will need to do so to complete this section. You can rerun the installer to select the missing component, or you can use the Visual Studio Quick Launch (Ctrl+Q). Type **Visual Studio Tools for Office** in the Quick Launch and select the Install option.

First, create a new console application named ExportDataToOfficeApp, activate the Add COM Reference dialog box by right-clicking the project in Solution Explorer, and select Add ► COM Reference. With the COM tab selected, find the COM library you want to use, which is Microsoft Excel 16.0 Object Library (see Figure 18-1).

![](bbox=[0.105, 0.435, 0.895, 0.685])

*Figure 18-1. The COM tab of the Add Reference dialog box will show you all registered COM libraries on your machine*

Once you select a COM library, the IDE will respond by generating a new assembly that contains .NET descriptions of COM metadata. Formally speaking, these are termed *interoperability assemblies* (or simply, *interop assemblies*). Interop assemblies do not contain any implementation code, except for a small amount that helps translate COM events to .NET Core events. However, these interop assemblies are useful, in that they shield your .NET Core code base from the complex underbelly of COM internals.