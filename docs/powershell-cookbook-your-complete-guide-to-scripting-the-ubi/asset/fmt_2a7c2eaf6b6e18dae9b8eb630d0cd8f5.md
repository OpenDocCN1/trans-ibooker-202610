
# Invoke-BinaryProcess

From *PowerShell Cookbook* (O'Reilly) by Lee Holmes (http://www.leeholmes.com/guide)

## SYNOPSIS

Invokes a process that emits or consumes binary data.

## EXAMPLE

```powershell
PS > Invoke-BinaryProcess binaryProcess.exe -RedirectOutput -ArgumentList "-emit" |
       Invoke-BinaryProcess binaryProcess.exe -RedirectInput -ArgumentList "-consume"
```

## Parameters

| Parameter | Alias | Type | Description |
|-----------|-------|------|-------------|
| `ProcessName` | | `[string]` | The name of the process to invoke |
| `RedirectInput` | `Input` | `[switch]` | Specifies that input to the process should be treated as binary |
| `RedirectOutput` | `Output` | `[switch]` | Specifies that the output of the process should be treated as binary |
| `ArgumentList` | | `[string]` | Specifies the arguments for the process |

## Script Body

```powershell
Set-StrictMode -Version 3

## Prepare to invoke the process
$processStartInfo = New-Object System.Diagnostics.ProcessStartInfo
$processStartInfo.FileName = (Get-Command $processname).Definition
$processStartInfo.WorkingDirectory = (Get-Location).Path
if($argumentList) { $processStartInfo.Arguments = $argumentList }
$processStartInfo.UseShellExecute = $false

## Always redirect the input and output of the process.
## Sometimes we will capture it as binary, other times we will
## just treat it as strings.
$processStartInfo.RedirectStandardOutput = $true
$processStartInfo.RedirectStandardInput = $true

$process = [System.Diagnostics.Process]::Start($processStartInfo)

## If we've been asked to redirect the input, treat it as bytes.
## Otherwise, write any input to the process as strings.
if($redirectInput)
{
    $inputBytes = @($input)
    $process.StandardInput.BaseStream.Write($inputBytes, 0, $inputBytes.Count)
    $process.StandardInput.Close()
}
else
{
    $input | % { $process.StandardInput.WriteLine($_) }
    $process.StandardInput.Close()
}

## If we've been asked to redirect the output, treat it as bytes.
## Otherwise, read any input from the process as strings.
if($redirectOutput)
{
    $byteRead = -1
    do
    {
        $byteRead = $process.StandardOutput.BaseStream.ReadByte()
        if($byteRead -ge 0) { $byteRead }
    } while($byteRead -ge 0)
}
else
{
    $process.StandardOutput.ReadToEnd()
}
```

## Discussion

When PowerShell launches a native application, one of the benefits it provides is allowing you to use PowerShell commands to work with the output. For example:

```powershell
PS > (ipconfig)[7]
   Link-local IPv6 Address . . . . . : fe80::20f9:871:8365:f368%8
PS > (ipconfig)[8]
   IPv4 Address. . . . . . . . . . . : 10.211.55.3
```

PowerShell enables this by splitting the output of the program on its newline characters, and then passing each line independently down the pipeline. This includes programs that use the Unix newline (`\n`) as well as the Windows newline (`\r\n`).

If the program outputs binary data, however, that reinterpretation can corrupt data as it gets redirected to another process or file. For example, some programs communicate between themselves through complicated binary data structures that cannot be modified along the way. This is common in some image editing utilities and other non-PowerShell tools designed for pipelined data manipulation.

We can see this through an example *BinaryProcess.exe* application that either emits binary data or consumes it. Here is the C# source code to the *BinaryProcess.exe* application:

```csharp
using System;
using System.IO;

public class BinaryProcess
{
    public static void Main(string[] args)
    {
        if(args[0] == "-consume")
        {
            using(Stream inputStream = Console.OpenStandardInput())
            {
                for(byte counter = 0; counter < 255; counter++)
                {
                    byte received = (byte) inputStream.ReadByte();
                    if(received != counter)
                    {
                        Console.WriteLine(
                            "Got an invalid byte: {0}, expected {1}.",
                            received, counter);
                        return;
                    }
                    else
                    {
                        Console.WriteLine(
                            "Properly received byte: {0}.", received, counter);
                    }
                }
            }
        }

        if(args[0] == "-emit")
        {
            using(Stream outputStream = Console.OpenStandardOutput())
            {
                for(byte counter = 0; counter < 255; counter++)
                {
                    outputStream.WriteByte(counter);
                }
            }
        }
    }
}
```

When we run it with the `-emit` parameter, PowerShell breaks the output into three objects:

```powershell
PS > $output = .\binaryprocess.exe -emit
PS > $output.Count
3
```

We would expect this output to contain the numbers 0 through 254, but we see that it does not:

```powershell
PS > $output | ForEach-Object { "------------";
    $_.ToCharArray() | ForEach-Object { [int] $_ } }
------------
0
1
2
3
4
5
6
7
8
9
------------
11
12
------------
14
15
16
17
18
19
20
21
22
(...)
255
214
220
162
163
165
8359
402
225
```

At number 10, PowerShell interprets that byte as the end of the line, and uses that to split the output into a new element. It does the same for number 13. Things appear to get even stranger when we get to the higher numbers and PowerShell starts to interpret combinations of bytes as Unicode characters from another language.

The solution resolves this behavior by managing the output of the binary process directly. If you supply the `-RedirectInput` parameter, the script assumes an incoming stream of binary data and passes it to the program directly. If you supply the `-RedirectOutput` parameter, the script assumes that the output is binary data, and likewise reads it from the process directly.

## See Also

[Recipe 1.2, "Run Programs, Scripts, and Existing Tools"](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# Chapter 3: Variables and Objects

## 3.0 Introduction

As touched on in [Chapter 2](#ch02.xhtml#pipelines), PowerShell makes life immensely easier by keeping information in its native form: *objects*. Users expend most of their effort in traditional shells just trying to resuscitate information that the shell converted from its native form to plain text. Tools have evolved that ease the burden of working with plain text, but that job is still significantly more difficult than it needs to be.

Since PowerShell builds on Microsoft's .NET Framework, native information comes in the form of .NET *objects*—packages of information and functionality closely related to that information.

Let's say that you want to get a list of running processes on your system. In other shells, your command (such as `tlist.exe` or `/bin/ps`) generates a plain-text report of the running processes on your system. To work with that output, you send it through a bevy of text processing tools—if you're lucky enough to have them available.

PowerShell's `Get-Process` cmdlet generates a list of the running processes on your system. In contrast to other shells, though, these are full-fidelity `System.Diagnostics.Process` objects straight out of the .NET Framework. The .NET Framework documentation describes them as objects that "provide access to local and remote processes, and enable you to start and stop local system processes." With those objects in hand, PowerShell makes it trivial for you to access properties of objects (such as their process name or memory usage) and to access functionality on these objects (such as stopping them, starting them, or waiting for them to exit).

## 3.1 Display the Properties of an Item as a List

### Problem
