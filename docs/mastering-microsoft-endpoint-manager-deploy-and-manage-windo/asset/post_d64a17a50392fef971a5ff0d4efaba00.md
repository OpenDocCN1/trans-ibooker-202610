
- 4. Click on **Search Universal Print for printers**. Your assigned cloud printers should pop up automatically:

![](img/a3e9f26159d3739e434fe676b2987c1d_569_0.png)

Figure 15.23 – Printers & scanners – Windows 10

- 5. Click on **Add device** to add the cloud-based printer to your virtual or physical desktop:

![](img/a3e9f26159d3739e434fe676b2987c1d_570_0.png)

Figure 15.24 – Add device

- 6. The printer has now been added and is ready to be tested:

Figure 15.25 – Printers & scanners – printer added

- 7. Open the cloud-based printer and click on Manage.

- 8. Click on Print a test page. The test page has been sent to the printer. You can open the print queue to see if something happens:

Figure 15.26 – Printers & scanners – Print a test page

- 9. The test print job has been sent to the printer.

- 10. If everything goes fine, the print job should be available and listed in the Universal Print admin portal too. You can find the jobs in the Universal Print portal by clicking on the printer, followed by Jobs. You should see the job with the status Completed. Great job:

![](img/a3e9f26159d3739e434fe676b2987c1d_572_0.png)

Figure 15.27 – Universal Print – print jobs

In the next section, you'll learn how to publish printers via Microsoft Endpoint Manager.

## Assigning and deploying cloud printers with Microsoft Endpoint Manager

In the previous section, you learned about all the basics – as well as the manual process – of assigning printers. This process is also possible to perform via Microsoft Endpoint Manager (as explained earlier) as a more enterprise-ready approach to, for example, assign hundreds of printers across the globe to your users:

- 1. First, download the Universal Print provisioning tool via this link: https://www.microsoft.com/en-us/download/details.aspx?id=101453.
- 2. Make the CSV list ready for deployment as Microsoft Endpoint Manager enrolment will use a CSV file as the source. You can find the printers.csv file in the tool as part of SamplePolicy.zip.
- 3. Add all the Universal Print printer share name and share IDs and whether the printer should become the default printer in the CSV file in the following order, SharedId, SharedName, IsDefault:

![](img/a3e9f26159d3739e434fe676b2987c1d_573_0.png)

Figure 15.28 – Universal Print – Microsoft Endpoint Manager CSV config file

- 4. Look up the printer share names and share IDs that you want to add via Microsoft Endpoint Manager:

![](img/a3e9f26159d3739e434fe676b2987c1d_573_1.png)

Figure 15.29 – Universal Print – printer overview

- 5. Generate the custom Win32 application package, which includes the new CSV file with the correct printer share names and IDs. You learned about creating a package in *Chapter 8, Application Management and Delivery*.
- 6. Download the Microsoft Win32 Content Prep Tool.
- 7. Unzip the tool, for example, on the C:\ drive (you can pick the folder of your choice) from https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool:

![](img/a3e9f26159d3739e434fe676b2987c1d_574_0.png)

Figure 15.30 – Intune – Win32 Content Prep Tool

- 8. Open PowerShell and change directory to the Intune Prep Tool location, for example, C:\Microsoft-Win32-Content-Prep-Tool-master:

![](img/a3e9f26159d3739e434fe676b2987c1d_574_1.png)

Figure 15.31 – PowerShell

- 9. Run .\IntuneWinAppUtil.exe and fill in the following requirements:
  - **Source folder**: The folder where the list of printers (`printers.csv`) and `InstallPolicy.cmd` files are present.
  - **Setup file**: The path of the `InstallPolicy.cmd` file (or any other script that will be used to copy the `printers.csv` file on users' devices).
  - **Output folder**: The folder where you would like the generated `intunewin` package file to be stored.
  - **Do you want to specify catalog folder (Y/N)**: Enter N.

![](img/a3e9f26159d3739e434fe676b2987c1d_575_0.png)

Figure 15.32 – PowerShell

- 10. The .intunewin package is ready to use within Microsoft Endpoint Manager:

![](img/a3e9f26159d3739e434fe676b2987c1d_575_1.png)

Figure 15.33 – The .intunewin package ready to use

- 11. Open the Microsoft Endpoint Manager console via http://endpoint.microsoft.com/. Then open the Apps menu and click on Add:

Figure 15.34 – Intune – Win32 configuration

- 12. Choose Windows app (Win32):

Figure 15.35 – Select app type

- 13. Click on **Select app package file** and browse to the `.intunewin` file output – for example, `InstallPolicy.intunewin` in the `C:\Microsoft-Win32-Content-Prep-Tool-master` folder:

![](img/a3e9f26159d3739e434fe676b2987c1d_577_0.png)

Figure 15.36 – App package file

- 14. Give the app a custom name such as `Install HP Printer`, as in the following example:

![](img/a3e9f26159d3739e434fe676b2987c1d_577_1.png)

Figure 15.37 – Add app

- 15. Change the line that follows in the **Install command** and **Uninstall command** fields. Make sure it's a **User** installed application:
  - InstallPolicy.cmd user install
  - InstallPolicy.cmd user uninstall:

![](img/a3e9f26159d3739e434fe676b2987c1d_578_0.png)

Figure 15.38 – Specify the commands to install and uninstall the app

- 16. Change the requirements in the screenshot to the baseline OS version and click **Next**. The baseline defines the minimum version of Windows to run in order to receive the printer assignment package:

![](img/a3e9f26159d3739e434fe676b2987c1d_579_0.png)

Figure 15.39 – Intune – Win32 configuration

- 17. Fill in the detection rule based on the CSV file that included the printers to be added. Add the following location in the **Path** field and `printers.csv` in the **File or folder** field: `%AppData%\UniversalPrintPrinterProvisioning\Configuration`.

- 18. Make sure Detection method is set to File or folder exists. Click OK:

![](img/a3e9f26159d3739e434fe676b2987c1d_580_0.png)

Figure 15.40 – Detection rule

- 19. Filter the list of printers in the Assignments menu to, for example, AD groups per department:

![](img/a3e9f26159d3739e434fe676b2987c1d_580_1.png)

Figure 15.41 – Filter the list of printers

- 20. Confirm the settings by clicking on Next and then Create:

![](img/a3e9f26159d3739e434fe676b2987c1d_581_0.png)

Figure 15.42 – Confirm the settings

- 21. Your printers will then be pushed to your physical and cloud desktops:

![](img/a3e9f26159d3739e434fe676b2987c1d_581_1.png)

Figure 15.43 – Intune – Win32 configuration

You've now finished configuring Universal Print and have become a real expert in printing to the cloud. This will make your life as an IT professional a lot easier while maintaining your printers in comparison with a traditional printing environment.

## Summary

In this chapter, you've learned about a new Microsoft 365 service called Universal Print, how to configure the service, and how to publish printers to your endpoints via Microsoft Endpoint Manager.

In the next chapter, we're going to talk about other managed devices that you can manage with Microsoft Endpoint Manager.

## Questions

- 1. What port needs to be open in the firewall in order to use Universal Print?
  - A. TCP - 443
  - B. TCP - 445
- 2. Are print jobs with Universal Print sent to the cloud encrypted?
  - A. Yes
  - B. No
- 3. Does Universal Print support zero-trust?
  - A. Yes, all traffic goes over the internet over SSL.
  - B. No.

## Answers

- 1. (A)
- 2. (A)
- 3. (A)

## Further reading

If you want to learn more about Universal Print after reading this chapter, please use the following free online resources:

- Universal Print – Tech Community via aka.ms/upcommunity: https://techcommunity.microsoft.com/t5/universal-print/ct-p/UniversalPrint
- Universal Print – Get started via aka.ms/updocs: https://docs.microsoft.com/en-us/universal-print/fundamentals/universal-print-getting-started

## Section 4: Tips and Tricks from the Field

Learn from the writers about the most common deployment- and networking-related errors they see in the field and learn how to counter and resolve them.

This part of the book comprises the following chapters:

- Chapter 16, Troubleshooting Microsoft Endpoint Manager
- Chapter 17, Troubleshooting Windows 365
- Chapter 18, Community Help
