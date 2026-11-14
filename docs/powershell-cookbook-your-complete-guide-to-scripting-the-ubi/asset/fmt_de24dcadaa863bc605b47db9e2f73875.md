
# Get-DiskUsage

## SYNOPSIS

Retrieve information about disk usage in the current directory and all subdirectories. If you specify the `-IncludeSubdirectories` flag, this script accounts for the size of subdirectories in the size of a directory.

## EXAMPLE

`PS > Get-DiskUsage`
Gets the disk usage for the current directory.

## EXAMPLE

`PS > Get-DiskUsage -IncludeSubdirectories`
Gets the disk usage for the current directory and those below it, adding the size of child directories to the directory that contains them.

## Parameters

```powershell
param(
    ## Switch to include subdirectories in the size of each directory
    [switch] $IncludeSubdirectories
)
```

## Script

```powershell
Set-StrictMode -Version 3

## If they specify the -IncludeSubdirectories flag, then we want to account
## for all subdirectories in the size of each directory
if($IncludeSubdirectories)
{
    Get-ChildItem -Directory |
        Select-Object Name,
            @{ Name="Size";
            Expression={ ($_ | Get-ChildItem -Recurse |
                Measure-Object -Sum Length).Sum + 0 } }
}
## Otherwise, we just find all directories below the current directory,
## and determine their size
else
{
    Get-ChildItem -Recurse -Directory |
        Select-Object FullName,
            @{ Name="Size";
            Expression={ ($_ | Get-ChildItem |
                Measure-Object -Sum Length).Sum + 0 } }
}
```

For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

- [Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [Recipe 3.14, “Add Custom Methods and Properties to Objects”](#ch03.xhtml#add_custom_methods_and_properties_to_obj)

# 20.9 Monitor a File for Changes

## Problem

You want to monitor the end of a file for new content.

## Solution

To monitor the end of a file for new content, use the `-Wait` parameter of the `Get-Content` cmdlet:

```powershell
Get-Content log.txt -Wait
```

## Discussion

The `-Wait` parameter of the `Get-Content` cmdlet acts much like the traditional Unix `tail` command with the `--follow` parameter. If you provide the `-Wait` parameter, the `Get-Content` cmdlet reads the content of the file but doesn’t exit. When a program appends new content to the end of the file, the `Get-Content` cmdlet returns that content and continues to wait.

If you want to monitor a large file, the initial output displayed by the `-Wait` parameter `Get-Content` might flood your screen or take a long time to complete. To read from the end of the file, use the `-Tail` parameter. For example, to start with the final 10 lines:

```powershell
Get-Content log.txt -Tail 10 -Wait
```

For more information about the `Get-Content` cmdlet, type `Get-Help Get-Content`. For more information about the `-Wait` parameter, type `Get-Help about_File​System_Provider`.

# 20.10 Get the Version of a DLL or Executable

## Problem

You want to examine the version information of a file.

## Solution

Use the `Get-Item` cmdlet to retrieve the file, and then access the `VersionInfo` property to retrieve its version information:

```powershell
PS > $file = Get-Item $pshome\pwsh.exe
PS > $file.VersionInfo

ProductVersion     FileVersion      FileName
--------------     -----------      --------
7.1.0 SHA: cbb7... 7.1.0.0          C:\Program Files\Windows...\pwsh.exe
```

## Discussion

One common task in system administration is identifying file and version information of installed software. PowerShell makes this simple through the `VersionInfo` property, which it automatically attaches to files that you retrieve through the `Get-Item` cmdlet. To generate a report for a directory, simply pass the output of `Get-ChildItem` to the `Select-Object` cmdlet, and use the `-ExpandProperty` parameter to expand the `VersionInfo` property.

```powershell
PS > Get-ChildItem $env:WINDIR |
     Select -Expand VersionInfo -ErrorAction SilentlyContinue

ProductVersion   FileVersion      FileName
--------------   -----------      --------
                                  C:\Windows\autologon.log
6.0.6000.16386   6.0.6000.1638... C:\Windows\bfsvc.exe
                                  C:\Windows\bootstat.dat
                                  C:\Windows\DtcInstall.log
6.0.6000.16386   6.0.6000.1638... C:\Windows\explorer.exe
6.0.6000.16386   6.0.6000.1638... C:\Windows\fveupdate.exe
6.0.6000.16386   6.0.6000.1638... C:\Windows\HelpPane.exe
6.0.6000.16386   6.0.6000.1638... C:\Windows\hh.exe
(...)
```

For more information about the `Get-ChildItem` cmdlet, see [Recipe 20.2](#ch20.xhtml#get_the_files_in_a_directory).

## See Also

- [Recipe 20.2, “Get the Files in a Directory”](#ch20.xhtml#get_the_files_in_a_directory)

# 20.11 Create a Directory

## Problem

You want to create a directory or file folder.

## Solution

To create a directory, use the `md` or `mkdir` function:

```powershell
PS > md NewDirectory


    Directory: C:\temp


Mode                LastWriteTime     Length Name
----                -------------     -----------
d----        4/29/2007   7:31  PM            NewDirectory
```

## Discussion

The `md` and `mkdir` functions are simple wrappers around the more sophisticated `New-Item` cmdlet. As you might guess, the `New-Item` cmdlet creates an item at the location you provide. To create a directory using the `New-Item` cmdlet directly, supply `Directory` to the `-Type` parameter.

```powershell
New-Item -Path C:\Temp\NewDirectory -Type Directory
```

The `New-Item` cmdlet doesn’t work against only the filesystem, however. Any providers that support the concept of items automatically support this cmdlet as well.

When you are working with the FileSystem provider, the `New-Item` cmdlet also supports symbolic links, junctions, and hard links.

For more information about the `New-Item` cmdlet, type `Get-Help New-Item`.

# 20.12 Remove a File or Directory

## Problem

You want to remove a file or directory.

## Solution

To remove a file or directory, use the `Remove-Item` cmdlet:

```powershell
PS > Test-Path NewDirectory
True
PS > Remove-Item NewDirectory
PS > Test-Path NewDirectory
False
```

## Discussion

The `Remove-Item` cmdlet removes an item from the location you provide. The `Remove-Item` cmdlet doesn’t work against only the filesystem, however. Any providers that support the concept of items automatically support this cmdlet as well.

**Note**

The `Remove-Item` cmdlet lets you specify multiple files through its `Path`, `Include`, `Exclude`, and `Filter` parameters. For information on how to use these parameters effectively, see [Recipe 20.6](#ch20.xhtml#find_files_that_match_a_pattern).

If the item is a container (for example, a directory), PowerShell warns you that your action will also remove anything inside that container. You can provide the `-Recurse` flag if you want to prevent this message.

For more information about the `Remove-Item` cmdlet, type `Get-Help Remove-Item`.

## See Also

- [Recipe 20.6, “Find Files That Match a Pattern”](#ch20.xhtml#find_files_that_match_a_pattern)

# 20.13 Rename a File or Directory

## Problem

You want to rename a file or directory.

## Solution

To rename an item in a provider, use the `Rename-Item` cmdlet:

```powershell
Rename-Item example.txt example2.txt
```

## Discussion

The `Rename-Item` cmdlet changes the name of an item.

Some shells let you rename multiple files at the same time. In those shells, the command looks like this:

```cmd
ren *.gif *.jpg
```

PowerShell doesn’t support this syntax, but provides even more power through its `-replace` operator. As a simple example, we can emulate the preceding command:

```powershell
Get-ChildItem *.gif | Rename-Item -NewName { $_.Name -replace '.gif$','.jpg' }
```
