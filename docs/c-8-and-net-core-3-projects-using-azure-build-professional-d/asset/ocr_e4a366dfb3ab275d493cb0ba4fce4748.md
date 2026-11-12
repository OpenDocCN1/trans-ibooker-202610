It's worth noting that, depending on how your service runs, and your own permissions, you may not have access to anywhere on the hard drive.

In my case, the full command looks like this:

```
c:\windows\system32\sc.exe create PhotoService binpath=
"C:\Dev\Packt\C-8-and-.NET-Core-3.0-Projects-Second-Edition\Chapter 7 -
Photo
Storage\PhotoStorage.WindowsService\bin\Debug\netcoreapp3.0\PhotoStorage.Wi
ndowsService.exe\" "C:\Dev\Packt\C-8-and-.NET-Core-3.0-Projects-Second-
Edition\Chapter 7 - Photo
Storage\PhotoStorage.WindowsService\bin\Debug\netcoreapp3.0\" " start= auto
```

The screenshot showing the preceding command is as follows:

![](bbox=[0.205, 0.39, 0.795, 0.62])

The command in the preceding screenshot can be explained as follows:

+   1. The spaces after the parameters are not cosmetic, but bizarrely necessary!
+   2. Any quotation marks inside the binpath parameter must be escaped.
+   3. These denote the opening and closing quotation marks for the binpath parameter.