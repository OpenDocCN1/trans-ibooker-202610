Please review the WinSCP GNU license, at https://winscp.net/eng/docs/license.

To install and register the external library on your workstation, follow these steps:

+   1. Download version 5.9.5 of the **Installation package**, or the .NET **assembly/COM library** of WinSCP from https://winscp.net/eng/download.php.
+   2. We recommend that you use the installation package, which will install the application and the necessary assemblies in a folder expected by the recipes in this chapter.
+   3. If you decided on using the installation package, run it, and then follow the instructions in the installation wizard to complete the installation.
+   4. If you prefer to download only the assembly, download the archive file, and then unzip it into the C:\Program Files (x86)\WinSCP\ folder.
+   5. After the installation has completed successfully, or after you placed the files in the specified folder, use Windows Explorer to navigate to the C:\SSIS2016Cookbook\Chapter13\Scripts folder.
+   6. Locate the Chapter13_GAC_WinSCPnet.bat command file, and open it in Notepad.
+   7. Inspect the file, and then close it.
+   8. In Windows Explorer, right-click the command file, and select **Execute as administrator...** from the shortcut menu.
+   9. In the **User Account Control** dialog click **OK** to allow the execution.
+   10. After the execution completes, press any key to close the command prompt window.

You are now ready to design the custom task.