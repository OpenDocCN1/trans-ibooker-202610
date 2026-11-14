
### See Also

[Recipe 5.15, “Convert Text Streams to Objects”](#ch05.xhtml#convert_text_streams_to_objects)

[Appendix B, `Regular Expression Reference`](#app02.xhtml#regular_expression_reference)

## 9.6 Parse and Manage Binary Files

### Problem

You want to work with binary data in a file.

### Solution

There are two main techniques when working with binary data in a file. The first is to read the file using the `Byte` encoding, so that PowerShell doesn’t treat the content as text. The second is to use the `BitConverter` class to translate these bytes back and forth into numbers that you more commonly care about.

[Example 9-2](#ch09.xhtml#get-characteristicsps1) displays the `Portable Executable (PE) header` of a Windows executable. The beginning section of any executable (a `.dll`, `.exe`, or any of several others) starts with a binary section known as the `Portable Executable (PE) header`—which contains a Common Object File Format (COFF) header. Part of this header includes characteristics about that file, such as whether the file is a DLL.

For more information about the PE header format, see [the PE header format specification](https://oreil.ly/u833N).

#### Example 9-2. Get-Characteristics.ps1

```
##############################################################################
##
## Get-Characteristics
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Get the file characteristics of a file in the PE Executable File Format.

.EXAMPLE

PS > Get-Characteristics $env:WINDIR\notepad.exe
IMAGE_FILE_LOCAL_SYMS_STRIPPED
IMAGE_FILE_RELOCS_STRIPPED
IMAGE_FILE_EXECUTABLE_IMAGE
IMAGE_FILE_32BIT_MACHINE
IMAGE_FILE_LINE_NUMS_STRIPPED

#>

param(
    ## The path to the file to check
    [Parameter(Mandatory = $true)]
    [string] $Path
)

Set-StrictMode -Version 3

## Define the characteristics used in the PE file header.
## Taken from:
## http://www.microsoft.com/whdc/system/platform/firmware/PECOFF.mspx
$characteristics = @{}
$characteristics["IMAGE_FILE_RELOCS_STRIPPED"] = 0x0001
$characteristics["IMAGE_FILE_EXECUTABLE_IMAGE"] = 0x0002
$characteristics["IMAGE_FILE_LINE_NUMS_STRIPPED"] = 0x0004
$characteristics["IMAGE_FILE_LOCAL_SYMS_STRIPPED"] = 0x0008
$characteristics["IMAGE_FILE_AGGRESSIVE_WS_TRIM"] = 0x0010
$characteristics["IMAGE_FILE_LARGE_ADDRESS_AWARE"] = 0x0020
$characteristics["RESERVED"] = 0x0040
$characteristics["IMAGE_FILE_BYTES_REVERSED_LO"] = 0x0080
$characteristics["IMAGE_FILE_32BIT_MACHINE"] = 0x0100
$characteristics["IMAGE_FILE_DEBUG_STRIPPED"] = 0x0200
$characteristics["IMAGE_FILE_REMOVABLE_RUN_FROM_SWAP"] = 0x0400
$characteristics["IMAGE_FILE_NET_RUN_FROM_SWAP"] = 0x0800
$characteristics["IMAGE_FILE_SYSTEM"] = 0x1000
$characteristics["IMAGE_FILE_DLL"] = 0x2000
$characteristics["IMAGE_FILE_UP_SYSTEM_ONLY"] = 0x4000
$characteristics["IMAGE_FILE_BYTES_REVERSED_HI"] = 0x8000

## Get the content of the file, as an array of bytes
$fileBytes = Get-Content $path -ReadCount 0 -AsByteStream

## The offset of the signature in the file is stored at location 0x3c.
$signatureOffset = [BitConverter]::ToUint32($fileBytes, 0x3c)

## Ensure it is a PE file
$signature = [char[]] $fileBytes[$signatureOffset..($signatureOffset + 3)]
if(($signature -join '') -ne "PE`0`0")
{
    throw "This file does not conform to the PE specification."
}

## The location of the COFF header is 4 bytes into the signature
$coffHeader = $signatureOffset + 4

## The characteristics data are 18 bytes into the COFF header. The
## BitConverter class manages the conversion of the 4 bytes into an integer.
$characteristicsData = [BitConverter]::ToInt32($fileBytes, $coffHeader + 18)

## Go through each of the characteristics. If the data from the file has that
## flag set, then output that characteristic.
foreach($key in $characteristics.Keys)
{
    $flag = $characteristics[$key]
    if(($characteristicsData -band $flag) -eq $flag)
    {
        $key
    }
}
```

### Discussion

For most files, this technique is the easiest way to work with binary data. If you actually modify the binary data, then you will also want to use the `Byte` encoding when you send it back to disk:

```
$fileBytes | Set-Content modified.exe -AsByteStream
```

For extremely large files, though, it may be unacceptably slow to load the entire file into memory when you work with it. If you begin to run against this limit, the solution is to use file management classes from the .NET Framework. These classes include `BinaryReader`, `StreamReader`, and others. For more information about working with classes from the .NET Framework, see [Recipe 3.8](#ch03.xhtml#work_with_net_objects). For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

### See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[Recipe 3.8, “Work with .NET Objects”](#ch03.xhtml#work_with_net_objects)

## 9.7 Create and Manage Temporary Files

### Problem

You want to create a file for temporary purposes or manage temporary files that already exist.

### Solution
